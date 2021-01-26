## Nginx 之旅

###  Nginx 简介

#### 什么是Nginx

>  Nginx 是一个高性能 HTTP 和 反向代理 服务器，特点占用内存少，并发能力强。在服务器领域性能比较强。

#### 代理

#### 正向代理

>  在客户端 （浏览器） 配置代理服务器，通过代理服务器进行互联网访问的过程。
>
>  举个简单例子：
>
>  例如：当你在 家玩 LOL 游戏，游戏里会有 网吧加成，你可以在本地设置一个网吧的代理，这样你在玩游戏时，也可以体验到 网吧的加成，虽然你不是真正的在网吧。
>
>  游戏是只认这个代理，它不看你是家还是在哪。

<img src="https://ae01.alicdn.com/kf/H994a957a9d3e4649b2ebf97c013fd9d9V.jpg">

#### 反向代理

> 如果服务器做了反向代理，所有的请求会发送到这个反向代理的服务器上，然后反向代理服务器去选择目标服务器获取数据返回给客户端，此时的反向代理服务器和目标服务器 对外是一个 服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。
>
> 举个例子：
>
> 例如： 你通过百度访问 一个网站， 这个网站的服务器中可能有多个Web服务，但是你直接通过域名就可以访问到指定的网站，这样其中就是反向代理帮你干的，不同的url 转向不通过得Web



<img src="https://ae01.alicdn.com/kf/H4d14c9f8d4454cbd99894c1c4e4add88d.jpg">

#### 小结

> 两者的区别在于代理的对象不一样： 正向代理是为客户端代理，反向代理是为服务端代理。

#### 负载均衡

> 请求数量大的时候，单个服务器，处理不了，需要增加服务器的数量， 将请求分发到各个服务器，然后 负载分发到不同的服务器上处理，最终返回结果数据。

<img src="https://ae01.alicdn.com/kf/H29757e1e33624d088d77b652265f9e7by.jpg">

#### 动静分离

> 为了加快网站的解析速度，可以将 动态资源 和 静态资源 分别放置不同服务器， 来加快解析速度。降低原来单个服务器压力。

<img src="https://ae01.alicdn.com/kf/H9c2b5f8af49b4146a61293c6a1f24835V.jpg">3

###  Nginx 常用命令

```nginx
1.查看版本
nginx -v

2.关闭 
nginx -s stop
3.退出
nginx -s quit
4.重启加载配置
nginx -s reload
5. 启动
./nginx
```

### Nginx 配置

#### Nginx 配置文件位置

> ​	nginx-1.17.2\conf     nginx.conf

#### Nginx 组成

> nginx 配置文件 由 三部分组成：
>
> 1. 全局块
> 2. events块
> 3. http块

#####  1. 全局块

> 从配置文件到 events 块之间的内容， 主要设置一些 影响nginx 服务器整体运行指令
>
> worker_processes  1;       配置的 并发处理量， 根据你自己的 服务器 CPU 核 来配置多少
>
> 
>
> 该部分配置主要影响Nginx全局，通常包括下面几个部分：
>
> 配置运行Nginx服务器用户（组）
> worker process数
> Nginx进程PID存放路径
> 错误日志的存放路径
> 配置文件的引入

##### 2. events 块

> events 块 的指令 主要影响nginx 服务器与用户的网络链接
>
> worker_connections  1024;      配置支持最大的连接数
>
> 
>
> 该部分配置主要影响Nginx服务器与用户的网络连接，主要包括：
>
> 设置网络连接的序列化
> 是否允许同时接收多个网络连接
> 事件驱动模型的选择
> 最大连接数的配置



##### 3.http块

> http  块 是 nginx 服务器配置最频繁的部分，代理，缓存核日志定义 等功能和第三方模块的配置在这块配置。
>
> 
>
> 定义MIMI-Type
> 自定义服务日志
> 允许sendfile方式传输文件
> 连接超时时间
> 单连接请求数上限

###### 3.1 server块

> 配置网络监听
> 基于名称的虚拟主机配置
> 基于IP的虚拟主机配置	

###### 3.2 location块

> location配置
> 请求根目录配置
> 更改location的URI
> 网站默认首页配置

### 反向代理操作

```nginx
    server {
        listen     80;    # 监听端口号， 服务器必须开启
        server_name  101.23.x.x;   #  这块为你的 服务器 公网 ip 地址

    	#设置url 转发
        location  ~ /vue/ {
        	# 设置代理
			proxy_pass  http:127.0.0.1:8080;
        }
   		#可设置多个url转发
        location  ~ /react/ {
			proxy_pass  http:127.0.0.1:8090;
        }
    }


访问：
101.23.x.x/vue/
101.23.x.x/react/  




    server
    {
        listen 80;  # 监听端口号
        server_name 111.229.51.6;   # 服务器ip 地址
        index index.html index.htm index.php;   #映射文件
        root  /www/wwwroot/Vue;   # 映射文件路径
        
    	# 设置url转发
        location /api/ {
        	#设置反向代理
        	proxy_pass http://localhost:3000/;
        }
    }
访问：
111.229.51.6/getList/
```

### 负载均衡

> 在 http块 进行配置， upstream 中配置多个服务器地址，来实现均衡负载。
>
> 然后通过在 serve块中 配置 反向代理的 地址为   负载均衡 name，这样就实现当多用户访问服务器时，减少了单个服务器压力大的情况，进行分发处理请求。

```nginx
upstream myserver{
    server  192.168.x.x:8080;
    server  192.168.x.x:8090;
}

    server
    {
        listen 80;
        server_name 111.229.51.6;
        index index.html index.htm index.php;
        root  /www/wwwroot/Vue;
        
        location /api/ {
        # 进行反向代理
        proxy_pass http://myserver;   #指定代理为 负载均衡name
        }
    }
```

#### 负载均衡 分配服务策略

- ##### 轮询

  ```nginx
  每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能自动剔除。
  
  # 默认 按 轮询的方式 执行 负载均衡。
  ```

- ##### weight 权重

  ```nginx
  # weight 代表权重默认为 1,权重越高被分配的客户端越多
  upstream myserver{
      server  192.168.x.x:8080 weight = 10;
      server  192.168.x.x:8090 weight = 20;
  }
  ```

- ##### ip_hash 

  ```nginx
  #每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问一个后端服务器
  upstream myserver{
      ip_hash;
      server  192.168.x.x:8080 ;
      server  192.168.x.x:8090 ;
  }
  ```

- ##### fair

  ```nginx
  #按后端服务器的响应时间来分配请求，响应时间短的优先分配。
  upstream myserver{
      server  192.168.x.x:8080 ;
      server  192.168.x.x:8090 ;
      fair;
  }
  ```

### Nginx 基本原理

> 在 开启Nginx 时， 默认会开启两个 线程， 一个 master线程， 一个 worker 线程。
>
> master 线程 是 用来 管理 worker 线程的。

<img src="https://ae01.alicdn.com/kf/H06475010634545b9b2ee6c073c563a6fT.jpg">

<center>Nginx 工作原理<center>

##### worker 是如何进行 工作

> 当 客户端发送了一个请求， 首先 进入的是 Master进程，而Master下面有多个worker进程， worker 的工作机制是 争抢， 哪个 worker 处理快，就会争抢到执行这个请求。

<img src="https://ae01.alicdn.com/kf/Hc497104b395247a8a3d7a2f46e75b2e1s.png">

##### 一个 master 和多个 woker 有好处

> （1）可以使用 nginx –s reload 热部署，利用 nginx 进行热部署操作
>
> （2）每个 woker 是独立的进程，如果有其中的一个 woker 出现问题，其他 woker 独立的，继续进行争抢，实现请求过程，不会造成服务中断	

##### 设置多少个 woker 合适

> worker 数和服务器的 cpu 数相等是最为适宜的

##### 连接数 worker_connection

- ###### 发送请求，占用了 woker 的几个连接数？

  > 2 或者 4 个,   
  >
  > 为什么是 2个 或者4个?
  >
  > 当发送一个请求，请求响应 为两个 连接，访问数据库 又需要两个连接。

- ###### nginx 支持的最大并发数是多少？

  > 普通的静态访问最大并发数是：
  >
  >  worker_connections * worker_processes /2
  >
  > 如果是 HTTP 作 为反向代理来说，最大并发数量是:
  >
  >  worker_connections *worker_processes/4。