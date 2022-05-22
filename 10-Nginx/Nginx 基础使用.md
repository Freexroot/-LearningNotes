

# **Nginx 基础使用**

-----

![image-20220520160640708](./Nginx.assets\image-20220520160640708.png)

---

## **目录结构**

进入Nginx的主目录我们可以看到这些文件夹

```
client_body_temp conf fastcgi_temp html logs proxy_temp sbin scgi_temp uwsgi_temp
```

其中这几个文件夹在刚安装后是没有的，主要用来存放运行过程中的临时文件

```
client_body_temp fastcgi_temp proxy_temp scgi_temp
```

```shell
conf
	ginx.conf #主配置文件，其他配置文件都被引入到这里
	#用来存放配置文件相关
html
	#用来存放静态文件的默认目录 html、css等
logs
	access.log #访问日志(没人每次访问都会记录)，人物时间对象，等等
	error.log #错误日志
	nginx.pid #进程号
sbin
	#nginx的主程序
	
*_temp #运行时，生成临时文件

```



## **基本运行原理**

![image-20220520161303269](./Nginx.assets\image-20220520161303269.png)

启动/sbin/nginx的主进程（Master）后，Master读取并校验配置文件，成功后，会开启多个子进程（Worker）接收响应请求，客户端发送请求被Worker接收后，会查找配置文件/html，到/html下查找对应的页面，从而返回结果



## **Nginx配置与应用场景**

## nginx.conf 配置文件核心部分讲解

```shell
worker_processes  1; # 启动的worker进程数，默认为1，按cpu核心个数写

events {
    worker_connections  1024; #每个worker进程的连接数
}


http {
    #include是引入关键字，这里引入了mime.types这个配置文件（同在conf目录下，mime.types是用来定义，请求返回的content-type,也就是告诉浏览器解析这个文件的方式，下载/视频播放/文本...）,一些里面没有的格式（比如mp5等），必须添加进mime.types，浏览器才能正确的解析，不然只能按照默认解析
    include       mime.types; 
    default_type  application/octet-stream; #mime.types里未定义的，使用默认格式application/octet-stream（二进制流）

    sendfile        on; #详情，见下文
    keepalive_timeout  65; #长链接超时时间
	
	#一个nginx可以启用多个vhost(虚拟主机)
	#虚拟主机：原本一台服务器只能对应一个站点，通过虚拟主机技术可以虚拟化成多个站点同时对外提供服务
    server {
        listen       80; #监听端口80
        server_name  localhost;  #域名，主机名（要写能解析的，不能随便写）

# http://域名 [ /xxoo/index.html ] ，配置[]里的信息
        location / { 
            root   html; #根目录指向本机 nginx/html 目录
            index  index.html index.htm; #默认页：转到 /index.html和index.htm文件
        }

        error_page   500 502 503 504  /50x.html; # 服务器错误码为500 502 503 504，转到"域名/50x.html"
        location = /50x.html {# 在html文件夹下找/50x.htm
            root   html;
        }

    }
}
```

**sendﬁle on;**

​	sendfile on; 使用linux的 sendfile(socket, file, len)高效网络传输，也就是数据0拷贝。

未开启sendﬁle

![image-20220520161309659](./Nginx.assets\image-20220520161309659.png)

 接收到信号后，根据配置文件找到对应的文件，把文件放到内存里之后再把内存里的文件传输到客户端

开启后

![image-20220520161314074](./Nginx.assets\image-20220520161314074.png)

接收到指令后，把文件直接传输给客户端，边读边传

**keepalive_timeout 65;**

keepalive_timeout 65;

**server**

![image-20220520161317840](./Nginx.assets\image-20220520161317840.png)

多域名绑定到同一个服务器（ip）里，nginx要管理，分流这些请求，调用特定的功能去处理它



### **多域名解析**

模拟域名解析

在 C:\Windows\System32\drivers\etc\hosts 下，绑定ip和域名

```shell
#虚拟机ip和任意域名,注意不支持通配符
192.168.x.x s.com
```

**hosts文件权限问题**

1.把只读去掉

<img src="./Nginx.assets\image-20220521100322480.png" alt="image-20220521100322480" style="zoom:50%;" />

2.修改权限

![image-20220521100448084](./Nginx.assets\image-20220521100448084.png)

3.之后就可以保存了

4.注意把权限改回去，以免被攻击

5.测试

```
#在windos的cmd，输入下面语句，测试是否成功
ping s.com
```



**多用户二级域名**

1.先创建任意文件夹，用来放被访问的文件

```
/www/www/index.html
/www/vod/index.html
```

2.域名解析

```shell
#在本地hosts修改，因为hosts文件是不支持通配符的，所以不能用 *.s.com
192.168.x.x www.s.com
192.168.x.x vod.s.com
```

```shell
#如果是购买域名的话，是可以使用通配符*的，就不用一个个配置了
*.s.com
```

2.修改配置文件nginx.conf，添加第二个server，并且修改root位置

```shell
    server {
        listen       80;
        server_name  www.s.com; #绑定域名，更多写法在下面的匹配规则里 

        location / {
            root   /www/www; #指向本地服务器,存放网页的地方
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;	#不用改，默认在nginx/html
        }
    }
    server {
        listen       80;
        server_name  vod.s.com;

        location / {
            root   /www/vod; #指向本地服务器,存放网页的地方
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```

3.重启nginx应用

```
systemctl reload nginx  #重启
systemctl status nginx  #查看状态
```

4.在网站登录测试

```
www.s.com
vod.s.com
```

总结：

想要在一台liunx里配置多个server的话

1. 域名一样，端口不同

2. 端口一样，域名不同

---



### **server_name匹配规则**

我们需要注意的是server_name匹配分先后顺序，写在前面的匹配上就不会继续往下匹配了。

**完整匹配**

我们可以在同一server_name中匹配多个域名

```
#空格分隔
server_name vod.s.com www1.s.com;
```

**通配符匹配**

```shell
server_name *.s.com #前匹配
server_name vod.*; #后匹配
```

**正则匹配**

```
server_name ~^[0-9]+\.s\.com$;
```



### 缓存问题



<img src="./Nginx.assets\image-20220521120415762.png" alt="image-20220521120415762" style="zoom: 67%;" />

1. chrome按F12去掉这个，可以不缓存

2. vod1.s.com/?xxxx,在后面加[ /?xxx ],浏览器会不读缓存访问
3. 强制刷新快捷键， 以谷歌为例
   1. 谷歌：Ctrl+Shift+R 或 Shift + F5 ，无缓存刷新（硬性重新加载）

----

### 短网址

作用，把冗长的网址转化成简短的网址，两个网站的跳转网页的一样的

![img](./Nginx.assets\1116722-20181214145416852-495729273.png)

1. 这当然要有个 短网址服务器 来实现，这个短网址服务器里有个数据库，记录（key短网址，value原网址）

过程：短网址->短网址服务器->查询短网址服务器数据库->返回原网址->重定向跳转到原网址

2. 在百度上短网址，有很多提供短网址生成服务的网站，可以直接使用（因为政策原因，有些要实名）。常见的有 【http://t.cn】和【https://dwz.cn】
3. 提供这服务的网站，也可以通过短网址的解析数量获取各原网址的访问数量等信息。

---

httpDNS

HttpDNS是使用HTTP协议向DNS服务器的80端口进行请求，代替传统的DNS协议向DNS服务器的53端口进行请求。也就是使用Http协议去进行dns解析请求，将服务器返回的解析结果（域名对应的服务器IP），直接向该IP发起对应的API服务请求，代替使用域名。

----



### **反向代理**

名词解释

```
正向代理代理：对于客户端来说，访问外网
反向代理代理：对于服务端来说，接收客户端请求
隧道式代理：请求和响应都要经过Nginx代理
DR模型:减少Nginx的压力，在响应给客户端的时候不用经过Nginx
lvs：更高性能的负载均衡器，功能简单，可以实现隧道式代理也可以实现DR模型
```



proxy_pass 

注意：proxy_pass 和 root 二选一配置

1. proxy_pass 是用来设置代理服务器的地址
   1. 具体的网址或者具体的主机.
      1. proxy_pass http://www.atguigu.com/; //转发
      2. proxy_pass http://atguigu.com/; //重定向
      3. proxy_pass 61.184.215.185
   2. 服务器集群



```shell
location / {
	proxy_pass http://www.atguigu.com;
	#root html;
	#index index.html index.htm
}
```



## **基于反向代理的负载均衡**



### **负载均衡策略**

### **轮询算法**

默认情况下使用轮询方式，多态服务器逐一转发，这种方式适用于无状态请求。

retry：重试机制，请求目标服务器错误，立刻重新发送给另一台服务器

和server在同一级下

```shell
upstream httpd {
	server 192.168.44.102:80; #经测试，不能用外网的网址
	server 192.168.43.103:80;
}
server {
    listen       80; 
    server_name  localhost;

    location / {
        proxy_pass http://httpds; #这里和上面的upstream名一样
        #root   /home/lizepeng/software/nginx/web1;
        #index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
        location = /50x.html {
        root   html;
    }
}

```

问题：

​	无法保存会话，因为不断轮询，服务器里的session无法共用。

​	可以无状态，即把session放在单独一台服务器管理（知识点：SpringSession,redis,token）

### **weight(权重)**

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。

```shell
upstream httpd {
	server 127.0.0.1:8050 weight=10 down;
	server 127.0.0.1:8060 weight=1;
	server 127.0.0.1:8060 weight=1 backup;
}
```

- down：表示当前的server暂时不参与负载
- weight：默认为1.weight越大，负载的权重就越大。
- backup： 备用，其它所有的非backup机器，down或者忙的时候，才使用backup机器。

下面几个是不常用的


### **ip_hash**

根据客户端的ip地址转发同一台服务器，可以保持会话。

落后了，比如手机ip是会不断改变的，无法解决会话问题。

### **least_conn**

最少连接访问，会自动把链接数多的服务器请求的转移到链接数少的服务器，不灵活

### **url_hash**

需要第三方插件，根据用户访问的url定向转发请求。

网址 ： xxx.com/哈希值

根据后面的哈希值来跳转到不同的服务器，但无法保存会话。

适用于：固定资源不在统一的服务器

### **fair**

需要第三方插件，根据后端服务器响应时间转发请求，有流量倾斜的风险





### **动静分离**

适用于中小型网站，把静态部分放在Nginx里

![image-20220521212300789](./Nginx.assets\image-20220521212300789.png)

#### **配置反向代理**

```shell
#proxy_pass 指向tomcat服务器
location / {
	proxy_pass http://127.0.0.1:8080;
	root html;
	index index.html index.htm;
}	
```



#### **增加每一个location**

```
location /css {
	root /usr/local/nginx/static;
	index index.html index.htm;
}
location /images {
	root /usr/local/nginx/static;
	index index.html index.htm;
}
location /js {
    root /usr/local/nginx/static;
	index index.html index.htm;
}
```



#### **使用一个location**

使用正则

**location 前缀**

----

/ 通用匹配，任何请求都会匹配到。

```shell
location / {
	root html;
	index index.html index.htm;
}
```

-----

= 精准匹配，不是以指定模式开头

-----

~ 正则匹配，区分大小写

-----

~* 正则匹配，不区分大小写

-----

^~ 非正则匹配，匹配以指定模式开头的location

-----



**location匹配顺序**

- 多个正则location直接按书写顺序匹配，成功后就不会继续往后面匹配

- 普通（非正则）location会一直往下，直到找到匹配度最高的（最大前缀匹配）

- 当普通location与正则location同时存在，如果正则匹配成功,则不会再执行普通匹配

- 所有类型location存在时，"="匹配 > "\^\~"匹配 > 正则匹配 > 普通（最大前缀匹配）


```shell
#js/img/css都会匹配到
location ~*/(css|img|js) {
	root /usr/local/nginx/static;
	index index.html index.htm;
}
```



#### **alias与root**

```
location /css {
	alias /usr/local/nginx/static/css;
	index index.html index.htm;
}
```

root用来设置根目录，而alias在接受请求的时候在路径上不会加上location。

1）alias指定的目录是准确的，即location匹配访问的path目录下的文件直接是在alias目录下查找的； 

2）root指定的目录是location匹配访问的path目录的上一级目录,这个path目录一定要是真实存在root指定目录下的；

 3）使用alias标签的目录块中不能使用rewrite的break（具体原因不明）；另外，alias指定的目录后面必须要加上\"/\"符

号！！ 

4）alias虚拟目录配置中，location匹配的path目录如果后面不带\"/\"，那么访问的url地址中这个path目录后面加不加\"/\"不影响访问，访问时它会自动加上\"/\"； 但是如果location匹配的path目录后面加上\"/\"，那么访问的url地址中这个path目录必须要加上\"/\"，访问时它不会自动加上\"/\"。如果不加上\"/\"，访问就会失败！

 5）root目录配置中，location匹配的path目录后面带不带\"/\"，都不会影响访问。



## **UrlRewrite**

### **rewrite语法格式及参数语法:**

```shell
rewrite是实现URL重写的关键指令，根据regex (正则表达式)部分内容，
重定向到replacement，结尾是flag标记。

rewrite		<regex> 	<replacement> 	[flag];
关键字		 正则 		替代内容 		flag标记

关键字：其中关键字error_log不能改变
正则：perl兼容正则表达式语句进行规则匹配
替代内容：将正则匹配的内容替换成replacement
flag标记：rewrite支持的flag标记，注意分号

rewrite参数的标签段位置：
server,location,if

flag标记说明：
last #本条规则匹配完成后，继续向下匹配新的location URI规则
break #本条规则匹配完成即终止，不再匹配后面的任何规则
redirect #返回302临时重定向，浏览器地址会显示跳转后的URL地址
permanent #返回301永久重定向，浏览器地址栏会显示跳转后的URL地址
```

实例

```shell
# $1 表示匹配第一个正则（括号里的内容）
rewrite ^/([0-9]+).html$ /index.jsp?pageNum=$1 break;
```



### **同时使用负载均衡**

#### **应用服务器防火墙配置**

#### **开启防火墙**

```shell
systemctl start firewalld
```



#### **重启防火墙**

```
systemctl restart firewalld
```



```shell
#ubuntu
sudo ufw status #查看防火墙状态，inactive是关闭，active是开启。
sudo ufw enable #开启防火墙。
sudo ufw disable #关闭防火墙。
sudo ufw reload  #重启ufw防火墙
```



#### **重载规则**

```
firewall-cmd \--reload
```



#### **查看已配置规则**

```
firewall-cmd \--list-all
```

```shell
#ubintu
sudo ufw status
```



#### **指定端口和ip访问**

```shell
firewall-cmd \--permanent \--add-rich-rule=\"rule family=\"ipv4\" source address=\"192.168.44.101\"
port protocol=\"tcp\" port=\"8080\" accept\"
```

```shell
#ubuntu，打开ip为192.168.x.x的8080端口
sudo ufw allow from 192.168.x.x to any port 8080
```



#### **移除规则**

```
firewall-cmd \--permanent \--remove-rich-rule=\"rule family=\"ipv4\" source
address=\"192.168.44.101\" port port=\"8080\" protocol=\"tcp\" accept\"
```

```shell
#关闭指定ip对应端口操作
sudo ufw delete allow from 192.168.x.x to any port 8080
```



#### **网关配置**

```
upstream httpds {
    server 192.168.44.102 weight=8 down;
    server 192.168.44.103:8080 weight=2;
    server 192.168.44.104:8080 weight=1 backup;
}
location / {
    rewrite ^/([0-9]+).html$ /index.jsp?pageNum=$1 redirect;
    proxy_pass http://httpds ;
}
```



### **防盗链配置**

Referer：Referer 是 HTTP 请求`header` 的一部分，当浏览器（或者模拟浏览器行为）向`web` 服务器发送请求的时候，头信息里有包含 Referer 。表示一个来源

防盗链：假如访问192.168.44.101时会显示一个照片，我只想通过192.168.44.101访问的用户可以看到，那么就可以通过Referer里的信息查看访问来源，来限制资源访问

```
valid_referers [none | blocked | server_names | strings ....] 域名/ip;
```

- none，检测 Referer 头域不存在的情况。
  - 如果Referer Header头的Referer存在且不符合设置的ip，则$invalid_referer=1.

- blocked，检测 Referer 头域的值被防火墙或者代理服务器删除或伪装的情况。这种情况该头域的值不以"http://" 或 "https://" 开头。
- server_names ，设置一个或多个 URL ，检测 Referer 头域的值是否是这些 URL 中的某一个。


在需要防盗链的location中配置

```shell
#在location下
location ~*/(css|img|js) {
	#访问（css|img|js）资源时，如果来源不是192.168.44.101(包括空),则$invalid_referer=1
    valid_referers 192.168.44.101;
    if ($invalid_referer) {
        return 401;
        
        #返回html/img/x.png
        #rewrite ^/ /img/x.png break;
    }
	root /usr/local/nginx/static;
	index index.html index.htm;
}
#自定义错误页面，如果不配置就用默认401错误页面
error_page 401 /401.html;
location = /401.html {
	#在html下要准备好401.html网页
	root html;
}
```

html里加<meta charset ="urf-8">可以解决中文乱码问题



#### **使用curl软件测试**

安装

```shell
yum install -y curl

#ubuntu
sudo apt install curl
```

只返回头信息

```
curl -I http://192.168.44.101/img/logo.png
```



#### **带引用**

```shell
#从http://baidu.com跳转到http://192.168.44.101/img/logo.png
curl -e "http://baidu.com" -I http://192.168.44.101/img/logo.png
```



## **高可用配置**

![image-20220522114041374](./Nginx.assets\image-20220522114041374-16531908452041.png)

在多台nginx服务器上都要安装Keepalived，有竞选机制，根据优先度来选择机器

vip:唯一的虚拟ip，用于代替真实ip，方便nginx服务器down时，可以快速切换。



### **安装Keepalived**

#### **1.安装包安装**

下载地址

```
https://www.keepalived.org/download.html#
```

使用 ./configure编译安装

如遇报错提示

```
configure: error:
!!! OpenSSL is not properly installed on your system. !!!
!!! Can not include OpenSSL headers files.!!!
```

安装依赖

```
yum install openssl-devel
```



#### **2.yum安装**

```
yum install keepalived
```



**3.ubuntu安装**

```
sudo apt-get install libssl-dev libpopt-dev daemon build-essential libssl-dev openssl libpopt-dev 
sudo apt install keepalived
```

刚安装完，配置文件目录/etc/keepalived/是空的，把下面的内容保存为文件keepalived.conf和chk_k8s_master.sh，放到该目录下



### **配置**

使用yum安装后配置文件在

/etc/keepalived/keepalived.conf

#### **最小配置**

第一台机器

```shell
! Configuration File for keepalived
global_defs {
	#router_id：标识，两台需不一样
    router_id lb111
}

#通讯协议，自定义名称"atguigu"
vrrp_instance atguigu {
	# 初始化状态-主机
    state MASTER
    # 虚拟 ip 绑定的网卡
    interface ens33
    # 此 ID 要与 Backup 配置一致
    virtual_router_id 51
    # 默认启动优先级，MASTER角色比BACKUP高，但要控制量，保证自身状态监测生效
    priority 100
    #间隔检测时间
    advert_int 1
    #同一组keepalived保存一致
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
    # 虚拟 ip 地址
        192.168.44.200
    }
}
```

第二台机器

```shell
! Configuration File for keepalived
global_defs {
	#二者名字不一样
    router_id lb110
}

vrrp_instance atguigu {
	#初始化状态-备份
    state BACKUP
    interface ens33
    virtual_router_id 51
    #优先级
    priority 50
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.44.200
    }
}
```

启动服务

```shell
systemctl start keepalived #启动
systemctl status keepalived #查看状态
systemctl restart keepalived #重启

```



## **Https证书配置**

### **不安全的http协议**

![image-20220520161341301](./Nginx.assets\image-20220520161341301.png)

对称加密不安全，加了计算因子也不安全

非对称加密

![image-20220522160746603](./Nginx.assets\image-20220522160746603.png)

非对称加密可以被中间人攻击

可以通过ca机构解决



### **openssl**

openssl包含：SSL协议库、应用程序以及密码算法库



### **自签名**

#### **OpenSSL**

系统内置

#### **图形化工具 XCA**

下载地址

```
https://www.hohnstaedt.de/xca/index.php/download
```



### **CA 签名**



## OneinStack

```shell
#官网
oneinstack.com
```

1. 购买服务器和域名，开放htts和https端口
2. 在OneinStack网站里，选择配置复制命令，在服务器粘贴命令后，等待下载安装，安装完之后的配置信息可以截图保存
3. 输入公网ip直接进入界面，测试成功
4. 域名和服务器ip绑定后，可以直接用域名访问
5. 在云平台申请SSl证书，之后下载对应证书的压缩包

```
xxx.域名.key
xxx.域名.pem
```

解压之后把两个文件上传到服务器

### 证书安装

```shell
server {
	#记得在安全组开放443端口（不行就把服务器里的端口也开放了）
	listen 443 ss1;
	server_name  www.aa.com; #需要将www.aa.com替换成证书绑定的域名，也可以用locahost。
	ss1 certificate  xxx.pem; #这里是证书路径，默认在conf下找
	ss1_ certificate_key  xxx.key  #这里是私钥路径
	
	#下面写location的配置
	...
}
```

### http协议跳转https

```shell

server {
	listen 80;
	server_name  www.upguigu.com upguigu.com;
	#加下面这些
	return 301 https://$server_name$request_uri;
}
```



## discuz论坛

```shell
https://discuz.net/
https://discuz.dismall.com/ #页面已迁移到这里
```

下载安装包后，发给服务器，放到 nginx/html 下，解压

```
unzip Discuz_X3.4_SC_UTF8_20220131.zip
```

upload是进入的地方，可以改名字,比如改成bbs

在网站输入[ https://域名/bbs/install ] ，进入在线安装界面

<img src="./Nginx.assets\image-20220522180608479.png" alt="image-20220522180608479" style="zoom: 50%;" />

 根据安装提示，给对应文件root权限

```shell
#生产环境不能这么粗暴
chmod -R 777 bbs
```

