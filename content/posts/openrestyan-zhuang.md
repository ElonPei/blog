---
title: OpenResty安装
date: 2018-01-01
---


转载自： http://jinnianshilongnian.iteye.com/blog/2186270

#### step1. 创建安装目录


```shell
mkdir -p /usr/servers
cd /usr/servers
```

<!-- more -->

#### step2. 安装依赖


```shell
yum install libreadline-dev libncurses5-dev libpcre3-dev libssl-dev perl
```

#### step3. 下载ngx_openresty-1.7.7.2.tar.gz并解压


```shell
wget http://openresty.org/download/ngx_openresty-1.7.7.2.tar.gz
tar -xzvf ngx_openresty-1.7.7.2.tar.gz
```

#### step4. 安装LuaJIT


```shell
cd bundle/LuaJIT-2.1-20150120/  
make clean && make && make install  
ln -sf luajit-2.1.0-alpha /usr/local/bin/luajit  
```

#### step5. 下载ngx_cache_purge模块，该模块用于清理nginx缓存


```shell
cd /usr/servers/ngx_openresty-1.7.7.2/bundle  
wget https://github.com/FRiCKLE/ngx_cache_purge/archive/2.3.tar.gz  
tar -xvf 2.3.tar.gz
```

#### step6. 下载nginx_upstream_check_module模块，该模块用于ustream健康检查


```shell
cd /usr/servers/ngx_openresty-1.7.7.2/bundle  
wget https://github.com/yaoweibin/nginx_upstream_check_module/archive/v0.3.0.tar.gz  
tar -xvf v0.3.0.tar.gz  
```

#### step7. 安装ngx_openresty


```shell
cd /usr/servers/ngx_openresty-1.7.7.2  
./configure --prefix=/usr/servers --with-http_realip_module  --with-pcre  --with-luajit --add-module=./bundle/ngx_cache_purge-2.3/ --add-module=./bundle/nginx_upstream_check_module-0.3.0/ -j2  
make && make install
```

--with***                安装一些内置/集成的模块

--with-http_realip_module  取用户真实ip模块

-with-pcre               Perl兼容的达式模块

--with-luajit              集成luajit模块

--add-module            添加自定义的第三方模块，如此次的ngx_che_purge

#### step8. 到/usr/servers目录下 


```shell
cd /usr/servers/  
ll   
```


会发现多出来了如下目录，说明安装成功

**/usr/servers/luajit **：luajit环境，luajit类似于java的jit，即即时编译，lua是一种解释语言，通过luajit可以即时编译lua代码到机器代码，得到很好的性能；

**/usr/servers/lualib**：要使用的lua库，里边提供了一些默认的lua库，如redis，json库等，也可以把一些自己开发的或第三方的放在这；




通过/usr/servers/nginx/sbin/nginx  -V 查看nginx版本和安装的模块
