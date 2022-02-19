---
categories:
  - Java
created_time: February 19, 2022 9:31 AM
date: 2018/01/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: HttpClient4.5.3源码浅析(HttpClient对象构造过程)
---


# 前言

由于现有的对`HttpClient`的工具类封装未形成统一的配置，针对线程池的配置也不全面，工具类的封装也没做到使用简洁合理回收，正好遇到`SpringMVC`向`SpringBoot`的迁移，于是对`HttpClient`进行一次重构，对`HttpClient`进行版本升级，并将相关配置分离出来管理。由于对`HttpClient`具体实现逻辑不是很清楚，所以有了这次的分析。

# 分析目标

- 学习`HttpClient`的代码架构，了解实现方式和具体的调用方式。
- 学习`HttpClient`对线程池的支持以及`IdleConnectionEvictor`类对线程池中失效连接的清除。
- 学习`HttpClientBuilder`中对`HttpClient`的构建过程。

# 整体结构

## HttpClient接口实现结构

![http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/Xnip2018-06-179_16-24-12.jpg](http://peierlong-blog.oss-cn-hongkong.aliyuncs.com/Xnip2018-06-179_16-24-12.jpg)

Xnip2018-06-179_16-24-12.jpg

`HttpClient`接口是`HttpClient`的核心接口，整个实现也是围绕着这个接口来做动作。

`HttpClient`接口有三个实现类，其中`AutoRetryHttpClient`和`DecompressingHttpClient`在4.3版本标记为过期类，故不做分析，主要来看`CloseableHttpClient`类。

`CloseableHttpClient`类是一个抽象类，它有三个子类，其中`AbstractHttpClient`为过期类，`MinimalHttpClient`是精简实现了，`InternalHttpClient`是主要实现。

## HttpClint的构造过程

首先调用`HttpClients`类的`createDefault()`，根据注释和返回都能看出采用的`CloseableHttpClient`为默认实现。

```
/**     * Creates {@link CloseableHttpClient} instance with default     * configuration.     */    public static CloseableHttpClient createDefault() {        return HttpClientBuilder.create().build();    }
```

其次通过`HttpClientBuilder.create()`创建`HttpClientBuilder`实例，并调用`build()`方法

```
public CloseableHttpClient build() {        // ... 照顾版面，省略中间各种配置代码        return new InternalHttpClient(                execChain,                connManagerCopy,                routePlannerCopy,                cookieSpecRegistryCopy,                authSchemeRegistryCopy,                defaultCookieStore,                defaultCredentialsProvider,                defaultRequestConfig != null ? defaultRequestConfig : RequestConfig.DEFAULT,                closeablesCopy);    }
```

我们看到，返回的`CloseableHttpClient`实例使用`InternalHttpClient`类实现。

## HttpClient对线程池的支持方式

HttpClientBuilder.java 类关于线程池部分源代码

```
public class HttpClientBuilder {    /**     * Assigns {@link HttpClientConnectionManager} instance.     */    public final HttpClientBuilder setConnectionManager(            final HttpClientConnectionManager connManager) {        this.connManager = connManager;        return this;    }    public CloseableHttpClient build() {        // ...        HttpClientConnectionManager connManagerCopy = this.connManager;        if (connManagerCopy == null) {            LayeredConnectionSocketFactory sslSocketFactoryCopy = this.sslSocketFactory;            if (sslSocketFactoryCopy == null) {                final String[] supportedProtocols = systemProperties ? split(                        System.getProperty("https.protocols")) : null;                final String[] supportedCipherSuites = systemProperties ? split(                        System.getProperty("https.cipherSuites")) : null;                HostnameVerifier hostnameVerifierCopy = this.hostnameVerifier;                if (hostnameVerifierCopy == null) {                    hostnameVerifierCopy = new DefaultHostnameVerifier(publicSuffixMatcherCopy);                }                if (sslContext != null) {                    sslSocketFactoryCopy = new SSLConnectionSocketFactory(                            sslContext, supportedProtocols, supportedCipherSuites, hostnameVerifierCopy);                } else {                    if (systemProperties) {                        sslSocketFactoryCopy = new SSLConnectionSocketFactory(                                (SSLSocketFactory) SSLSocketFactory.getDefault(),                                supportedProtocols, supportedCipherSuites, hostnameVerifierCopy);                    } else {                        sslSocketFactoryCopy = new SSLConnectionSocketFactory(                                SSLContexts.createDefault(),                                hostnameVerifierCopy);                    }                }            }            @SuppressWarnings("resource")            final PoolingHttpClientConnectionManager poolingmgr = new PoolingHttpClientConnectionManager(                    RegistryBuilder.<ConnectionSocketFactory>create()                        .register("http", PlainConnectionSocketFactory.getSocketFactory())                        .register("https", sslSocketFactoryCopy)                        .build(),                    null,                    null,                    dnsResolver,                    connTimeToLive,                    connTimeToLiveTimeUnit != null ? connTimeToLiveTimeUnit : TimeUnit.MILLISECONDS);            if (defaultSocketConfig != null) {                poolingmgr.setDefaultSocketConfig(defaultSocketConfig);            }            if (defaultConnectionConfig != null) {                poolingmgr.setDefaultConnectionConfig(defaultConnectionConfig);            }            if (systemProperties) {                String s = System.getProperty("http.keepAlive", "true");                if ("true".equalsIgnoreCase(s)) {                    s = System.getProperty("http.maxConnections", "5");                    final int max = Integer.parseInt(s);                    poolingmgr.setDefaultMaxPerRoute(max);                    poolingmgr.setMaxTotal(2 * max);                }            }            if (maxConnTotal > 0) {                poolingmgr.setMaxTotal(maxConnTotal);            }            if (maxConnPerRoute > 0) {                poolingmgr.setDefaultMaxPerRoute(maxConnPerRoute);            }            connManagerCopy = poolingmgr;        }        // ...    }}
```

可以看到`HttpClientBuilder`提供`setConnectionManager()`方法来传入一个`HttpClientConnectionManager`类型，而`HttpClientBuilder`可以通过`HttpClients.custom()`方法获得。

如果我们不传入自定义的`HttpClientConnectionManager`，在执行`build()`方法时会创建一个默认的线程池，代码参考`HttpClientBuilder`的`build()`方法。

## HttpClient对线程池失效链接的的处理

在4.4版本，提供了`IdleConnectionEvictor`类进行对失效线程和超时闲置线程的处理。主要实现代码如下：

```
this.thread = this.threadFactory.newThread(new Runnable() {            @Override            public void run() {                try {                    while (!Thread.currentThread().isInterrupted()) {                        Thread.sleep(sleepTimeMs);                        connectionManager.closeExpiredConnections();                        if (maxIdleTimeMs > 0) {                            connectionManager.closeIdleConnections(maxIdleTimeMs, TimeUnit.MILLISECONDS);                        }                    }                } catch (final Exception ex) {                    exception = ex;                }            }        });
```

简单来讲，实现原理是起一个后台线程，心跳检测，调用`connectionManager.closeExpiredConnections()`方法来进行失效回收，在`HttpClientBuilder`中有两个参数来控制是否启用失效回收。

```
    private boolean evictExpiredConnections;    private boolean evictIdleConnections;
```

在`build()`方法中调用如下

```
    if (evictExpiredConnections || evictIdleConnections) {                final IdleConnectionEvictor connectionEvictor = new IdleConnectionEvictor(cm,                        maxIdleTime > 0 ? maxIdleTime : 10, maxIdleTimeUnit != null ? maxIdleTimeUnit : TimeUnit.SECONDS);                closeablesCopy.add(new Closeable() {                    @Override                    public void close() throws IOException {                        connectionEvictor.shutdown();                    }                });                connectionEvictor.start();            }
```

# remark

至此，对`HttpClient`的构造过程有所了解，下一篇主要学习一下HttpClient的execute()方法的执行过程。