---
title: HttpClient 4.5.3 源码浅析( HttpClient 的 execute 方法执行过程)
date: 2018-01-01
tags:
  - Java
---


上一遍总结的`HttpClient`对象的构造过程，本篇主要总结`HttpClient`对象的`execute()`方法的执行流程，我们先看整体的时序图（省略部分细节），然后一步步分析。当不知道流程的时候走debug也是不错的选择。


<!-- more -->


![Xnip2018-06-180_15-08-22.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/Xnip2018-06-180_15-08-22.jpg)


根据上一篇文章我们知道`HttpClient`实例是使用的子类`CloseableHttpClient`类初始化而来，所以我们看子类的`execute()`方法的源代码。


```Java
  /**
   * {@inheritDoc}
   */
  @Override
  public CloseableHttpResponse execute(
          final HttpUriRequest request) throws IOException, ClientProtocolException {
      return execute(request, (HttpContext) null);
  }
  
  /**
   * {@inheritDoc}
   */
  @Override
  public CloseableHttpResponse execute(
          final HttpUriRequest request,
          final HttpContext context) throws IOException, ClientProtocolException {
      Args.notNull(request, "HTTP request");
      return doExecute(determineTarget(request), request, context);
  }
  
  protected abstract CloseableHttpResponse doExecute(HttpHost target, HttpRequest request,
          HttpContext context) throws IOException, ClientProtocolException;

```


我们看到调用链调用到了抽象方法`doExecute()`，上一篇中，我们知道`HttpClient`对象构建最后，是使用的`CloseableHttpClient`的子类`InternalHttpClient`创建的对象，所以我们来看`InternalHttpClient`类的`doExecute()`方法的具体实现。



```Java
  @Override
  protected CloseableHttpResponse doExecute(
          final HttpHost target,
          final HttpRequest request,
          final HttpContext context) throws IOException, ClientProtocolException {
      Args.notNull(request, "HTTP request");
      HttpExecutionAware execAware = null;
      if (request instanceof HttpExecutionAware) {
          execAware = (HttpExecutionAware) request;
      }
      try {
          final HttpRequestWrapper wrapper = HttpRequestWrapper.wrap(request, target);
          final HttpClientContext localcontext = HttpClientContext.adapt(
                  context != null ? context : new BasicHttpContext());
          RequestConfig config = null;
          if (request instanceof Configurable) {
              config = ((Configurable) request).getConfig();
          }
          if (config == null) {
              final HttpParams params = request.getParams();
              if (params instanceof HttpParamsNames) {
                  if (!((HttpParamsNames) params).getNames().isEmpty()) {
                      config = HttpClientParamConfig.getRequestConfig(params, this.defaultConfig);
                  }
              } else {
                  config = HttpClientParamConfig.getRequestConfig(params, this.defaultConfig);
              }
          }
          if (config != null) {
              localcontext.setRequestConfig(config);
          }
          setupContext(localcontext);
          final HttpRoute route = determineRoute(target, wrapper, localcontext);
          return this.execChain.execute(route, wrapper, localcontext, execAware);
      } catch (final HttpException httpException) {
          throw new ClientProtocolException(httpException);
      }
  }
```


以上代码中


1. `HttpExecutionAware`类是用于接收阻塞I/O操作的通知。

2. `HttpRequestWrapper`类是`HttpRequest`接口的包装类。

3. `HttpClientContext`类实现`HttpContext`接口，表示HTTP进程的执行状态。

4. `RequestConfig`请求配置类，初始化后赋值到了上下文`HttpClientContext`中。

5. `HttpRoute`为路由类。想更深了解可以参考[这篇文章](https://blog.csdn.net/zjysource/article/details/52945494)。


---


接下来主要逻辑在`MainClientExec`的`execute()`方法，涉及到有关https的auth的内容暂时不做分析。



```Java
  if (request instanceof HttpEntityEnclosingRequest) {
      RequestEntityProxy.enhance((HttpEntityEnclosingRequest) request);
  }
```


如果是`HttpEntityEnclosingRequest`类型的requet，则使用Entity代理类进行加强，主要用途是对entity的回收等操作。



```Java
  Object userToken = context.getUserToken();
```


这是`HttpClient`标识，默认为null，为了保证用户使用链接的唯一性。



```Java
  final ConnectionRequest connRequest = connManager.requestConnection(route, userToken);
```


得到一个由`connManager`管理其生命周期的`ConnectionRequest`



```Java
  if (execAware != null) {
          if (execAware.isAborted()) {
              connRequest.cancel();
              throw new RequestAbortedException("Request aborted");
          } else {
              execAware.setCancellable(connRequest);
          }
      }
```


`execAware`也实际表示一个封装请求。把当前链接赋值给`execAware`持有。



```Java
      final RequestConfig config = context.getRequestConfig();
```


从`context`中取出请求配置类。



```Java
      final HttpClientConnection managedConn;
      try {
          final int timeout = config.getConnectionRequestTimeout();
          managedConn = connRequest.get(timeout > 0 ? timeout : 0, TimeUnit.MILLISECONDS);
      } catch(final InterruptedException interrupted) {
          Thread.currentThread().interrupt();
          throw new RequestAbortedException("Request aborted", interrupted);
      } catch(final ExecutionException ex) {
          Throwable cause = ex.getCause();
          if (cause == null) {
              cause = ex;
          }
          throw new RequestAbortedException("Request execution failed", cause);
      }
```


通过`connRequest`得到一个`HttpClientConnection`，`HttpClientConnection` 可用于通过指定路由路径进行通信。



```Java
  context.setAttribute(HttpCoreContext.HTTP_CONNECTION, managedConn);

      if (config.isStaleConnectionCheckEnabled()) {
          // validate connection
          if (managedConn.isOpen()) {
              this.log.debug("Stale connection check");
              if (managedConn.isStale()) {
                  this.log.debug("Stale connection detected");
                  managedConn.close();
              }
          }
      }
```


1. 把`managedConn`放入`context`上下文中

2. 对失效链接进行校验



```Java
final ConnectionHolder connHolder = new ConnectionHolder(this.log, this.connManager, managedConn);
```


创建一个链接持有者。



```Java
if (execAware != null) {
              execAware.setCancellable(connHolder);
          }
```


使`execAware`请求`request`拥有链接持有者，这样用户可以对`request`可以直接对持有者进行操作。


```Java
  HttpResponse response;
          for (int execCount = 1;; execCount++) {

              if (execCount > 1 && !RequestEntityProxy.isRepeatable(request)) {
                  throw new NonRepeatableRequestException("Cannot retry request " +
                          "with a non-repeatable request entity.");
              }

              if (execAware != null && execAware.isAborted()) {
                  throw new RequestAbortedException("Request aborted");
              }

              if (!managedConn.isOpen()) {
                  this.log.debug("Opening connection " + route);
                  try {
                      establishRoute(proxyAuthState, managedConn, route, request, context);
                  } catch (final TunnelRefusedException ex) {
                      if (this.log.isDebugEnabled()) {
                          this.log.debug(ex.getMessage());
                      }
                      response = ex.getResponse();
                      break;
                  }
              }
              final int timeout = config.getSocketTimeout();
              if (timeout >= 0) {
                  managedConn.setSocketTimeout(timeout);
              }

              if (execAware != null && execAware.isAborted()) {
                  throw new RequestAbortedException("Request aborted");
              }

              if (this.log.isDebugEnabled()) {
                  this.log.debug("Executing request " + request.getRequestLine());
              }

              // 。。。 省略auth相关

              response = requestExecutor.execute(request, managedConn, context);

              // The connection is in or can be brought to a re-usable state.
              if (reuseStrategy.keepAlive(response, context)) {
                  // Set the idle duration of this connection
                  final long duration = keepAliveStrategy.getKeepAliveDuration(response, context);
                  if (this.log.isDebugEnabled()) {
                      final String s;
                      if (duration > 0) {
                          s = "for " + duration + " " + TimeUnit.MILLISECONDS;
                      } else {
                          s = "indefinitely";
                      }
                      this.log.debug("Connection can be kept alive " + s);
                  }
                  connHolder.setValidFor(duration, TimeUnit.MILLISECONDS);
                  connHolder.markReusable();
              } else {
                  connHolder.markNonReusable();
              }
              // 。。。 省略auth相关、
          }
```


这个代码块放在一个循环里边，循环的正常流程的终止条件在`needAuthentication()`判断中，暂不做分析。我们来对以上代码进行一个简要的分析：

1. 判断在第二次循环以后是否可以重复读，如果不可以重复读，则抛出异常。(不可重复读的指的是流，像`StringEntity` `FileEntity`这样的都是可以重复读的)

2. 好几处地方都对`execAware`真正的`request`的是否终止`isAborted()`方法做校验，如果终止了，则抛出异常。有了这个我们就可以随时取消请求了。

3. 如果链接不是open状态的，那么使用`route`路由对象重新建立一次链接。

4. 对链接进行超时设置。

5. 使用请求执行器`requestExecutor`执行请求得到`response`对象。

6. 根据消息头判断链接是否要保持长链接，如果是，标记`connHolder`为可重用的，如果否，则标记为不可重用的。


> 注：在执行请求的时候，用到的`request`是`HttpRequestWrapper`包装类，使用包装类是防止真正的请求操作时发生改变。



```Java
  if (userToken == null) {
              userToken = userTokenHandler.getUserToken(context);
              context.setAttribute(HttpClientContext.USER_TOKEN, userToken);
          }
          if (userToken != null) {
              connHolder.setState(userToken);
          }
```


把当前用户标识保存给链接持有者。


```Java
      // check for entity, release connection if possible
      final HttpEntity entity = response.getEntity();
      if (entity == null || !entity.isStreaming()) {
          // connection not needed and (assumed to be) in re-usable state
          connHolder.releaseConnection();
          return new HttpResponseProxy(response, null);
      } else {
          return new HttpResponseProxy(response, connHolder);
      }
```


1. 从返回的`respnse`中得到`HttpEntity`，校验实体如果是空或者不是一个流，则释放链接。

2. 使用`HttpResponseProxy`代理类对response进行代理，如果读取完了响应，那么这个响应就会关闭。


---

至此，`HttpClient`对象的`execute()`方法的简要的执行流程基本上就分析完毕。
