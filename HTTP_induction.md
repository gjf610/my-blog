## 浅析URL

### WWW = URL + HTTP + HTML
#### IP(Internet Protocal)
主要约定了两件事：
* 一、如何定位一台设备
* 二、如花封装数据报文，用来跟其他设备交流

具体内容我们不关心

IP分为内网和外网
* 外网：路由器连上电信的服务器，路由器就会有一个“外网IP”。这就是用户在互联网的地址。但是如果你重启路由器，那么你很有可能被重新分配一个“外网IP”，也就是说你的路由器没有“固定的外网IP”。
* 内网：路由器会在当前创建一个内网，内网中的设备使用“内网IP”，一般来说这个IP的格式都是192.168.XXX.XXX。一般路由器会给自己分配一个好记的“内网IP”，如192.168.1.1。然后路由器会给每一个内网中的设备分配一个不同的内网“内网IP”。

路由器的功能
* 现在路由器有两个IP，一个外网IP和一个内网IP
* 内网中的设备可以互相访问，但是不能直接访问外网
* 内网设备想要访问外网，就必须经过路由器中转
* 外网中的设备可以互相访问，但是无法访问你的内网
* 外网设备想要把内容送到内网，也必须通过路由器
* 就是说内网和外网就像两个隔绝的空间，无法互通，唯一的联通点就是路由器
* 所以路由器有时候也被叫做“网关”

几个特殊的IP
* 127.0.01表示自己
* localhost通过hosts指定为自己
* 0.0.0.0不表示任何设备
#### 端口
一台机器可以提供很多服务，每一个服务一个号码，这个号码就叫端口号port
* HTTP服务最好使用80端口
* HTTPS服务最好使用443端口
* FTP服务最好使用21端口
* 一个有65535个端口

规则
* 0到1023（2的10次方减1）号端口是留给系统使用的
* 你只有拥有管理员权限后，才能使用者1024个端口
* 其他端口可以给普通用户使用
* 一个端口如果被占用，你就只能换一个端口
#### 总而言之，IP和端口缺一不可

### 域名

#### 域名就是对IP的别称
* 一个域名可以对应不同IP
* 这个叫做负载均衡，防止一台机器扛不住
* 一个IP可以对应不同域名
* 这个叫做共享主机，穷开发会这么做

域名和IP是怎么对应起来的

  通过DNS(Domain Name System)

输入一个网址的baidu.com
1. 你的Chrome浏览器会向电信服务商提供的DNS服务器询问baidu.com对应的IP
2. 电信服务商会回答一个IP(具体过程略)
3. 然后Chrome才会向对应IP的80/443端口发送请求
4. 请求内容是查看baidu.com的首页

题外话
* WWW
  
  WWW.xiedaimala.com和xiedaimala.com是同一个域名吗？不是
* 他们是什么关系
  com是顶级域名
  xiedaimala.com是二级域名（俗称一级域名）
  www.xiedaimala.com是三级域名（俗称二级域名）
  他们是父子关系
  github.io把子域名xxx.github.io免费给你使用
  所有你应该知道www.xiedaimala.com和xiedaimala.com可以不是同一家公司，也可以是

### 如何请求不同的页面
1. 路径
2. 查询参数 同一个页面，不同内容
3. 锚点 同一个内容，不同位置

### URL
协议 +  域名或IP + 端口号 + 路径 + 查询字符串 + 锚点
https://www.baidu.com/s?wd=DNS#ie

### HTTP (Hyper Text Transfer Protocol)
基于TCP和IP两个协议开发的







资料来源： &copy;饥人谷