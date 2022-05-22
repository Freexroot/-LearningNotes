

# **Nginx 安装部署**

---

![image-20220519215624413](.\Nginx.assets\image-20220519215624413.png)



## **虚拟机安装**



- 虚拟机：VMware workstation 16

- 操作系统：CentOS 7.4


下载地址：<span class="underline"><https://vault.centos.org/centos/7.4.1708/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso></span>

### **虚拟机安装CentOS7.4**

#### **1 新建虚拟机**

![image-20220519215934891](.\Nginx.assets\image-20220519215934891.png)

#### **2 选择典型**

image-20220312153100199

#### **3 选择CentOS镜像**

我们在这次学习时使用mini班操作系统镜像，安装速度快，也去除了我们用不到的软件。

![image-20220519215944485](.\Nginx.assets\image-20220519215944485.png)

#### **4 存储位置**

![image-20220519220003656](.\Nginx.assets\image-20220519220003656.png)

#### **5 虚拟机磁盘配置**

![image-20220519220010392](.\Nginx.assets\image-20220519220010392.png)

#### **6 自定义其他配置**

![image-20220519220018659](.\Nginx.assets\image-20220519220018659.png)

在自定义硬件中，我们可以再次配置虚拟机的内存、cpu等硬件属性，在当前Nginx学习阶段硬件配置不需要过高，

默认单核cpu、1G内存即可。

### **学习时的电脑配置**

内存：建议8G以上

磁盘：建议使用SSD

CPU：4核以上主流即可

### **系统安装**

#### **1 虚拟机配置完成之后进入系统安装界面**

![image-20220519220034903](.\Nginx.assets\image-20220519220034903.png)

出现此界面后敲"回车"进入安装程序

#### **2 选择安装语言**

![image-20220519220041660](.\Nginx.assets\image-20220519220041660.png)

#### **3 分区选择**

虽然默认会自动帮我们格式化磁盘，但也需要点击确认一下

![image-20220519220047563](.\Nginx.assets\image-20220519220047563.png)

![image-20220519220055818](.\Nginx.assets\image-20220519220055818.png)

点击左上角完成即可

####  **4 开始安装**

开始

![image-20220519220101949](.\Nginx.assets\image-20220519220101949.png)

安装过程中我们可以设置密码

![image-20220519220110234](.\Nginx.assets\image-20220519220110234.png)

##### **安装完成**

当出现"重启"按钮时，说明系统已经安装完成

![image-20220519220703512](.\Nginx.assets\image-20220519220703512.png)

重启后的样子

![image-20220519220708468](.\Nginx.assets\image-20220519220708468.png)

至此，我们在VMware中对CentOS的基本安装已经完成。

##### **Linux配置**

##### 配置上网

修改配置网卡配置文件

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

![image-20220519220724246](.\Nginx.assets\image-20220519220724246.png)

修改 ONBOOT=yes

重启网络服务

```
systemctl restart network
```

- 测试


```
ping qq.com
```

![image-20220519220742325](.\Nginx.assets\image-20220519220742325.png)

至此，我们的虚拟机就可以访问互联网了。

##### **配置静态ip**

之前的网络配置是使用dhcp方式分配ip地址，这种方式会在系统每次联网的时候分配一个ip给我们用，也就是说有可

能系统下次启动的时候ip会变，这样非常不方便我们管理。

配置静态ip首先需要打开网卡配置文件

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

- 修改启动协议 BOOTPROTO=static

- 手动配置ip地址


```
IPADDR=192.168.44.101
NETMASK=255.255.255.0
GATEWAY=192.168.44.1
DNS1=8.8.8.8
```

根据大家自己的环境，ip地址可能略有不同。

- 接下来重启网络服务

```
systemctl restart network
```

重启之后 xshell可能无响应，这是因为ip变了，修改xshell配置中的ip重新连接即可。

完整配置截图如下

![](.\Nginx.assets\image-20220519220938056.png)

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=10ac735e-0b8f-4b19-9747-ff28b58a1547
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.44.101
NETMASK=255.255.255.0
GATEWAY=192.168.44.1
DNS1=8.8.8.8
```



##### **不能上网的错误排查**

- •Vmware中网关是否正确


​		•直接ping ip是否能通（物理连接排查）

​		•使用和老师一样版本的软件

​		•卸载重装最快

##### **一些公网DNS服务器**

##### **阿里**

```
223.5.5.5
223.6.6.6
```

##### **腾讯**

```
119.29.29.29
182.254.118.118
```

##### **百度**

```
180.76.76.76
```

##### **114DNS**

```
114.114.114.114
114.114.115.115
```

##### **谷歌**

```
8.8.8.8
8.8.4.4
```

## **Nginx的安装**

#### **版本区别**

常用版本分为四大阵营

Nginx开源版（免费法，纯净版，最干净的版本）

- <http://nginx.org/>

Nginx plus 商业版（收费，加强，全面）

- [https://www.nginx.com](https://www.nginx.com/)

openresty（整合了lua脚本）

- <span class="underline"><http://openresty.org/cn/></span>

Tengine（免费，增强）

- <http://tengine.taobao.org/>

#### **编译安装**

[官方下载地址](http://nginx.org/en/download.html)

![image-20220519225751549](./Nginx.assets\image-20220519225751549.png)



之后上传到liunx里

- 这时候你可以保存虚拟机状态（设置快照或者克隆），方便系统遇到不可挽回的错误时，及时回档，节省时间。

下面是快照的位置，即可以设置多个快照,也可以回到之前的快照

![image-20220519230656195](./Nginx.assets\image-20220519230656195.png)



解压Nginx包，并安装

```shell
tar -zxvf  nginx-1.21.6.tar.gz #解压到当前目录
cd nginx-1.21.6 #进入解压后的文件夹
./configure --prefix=/usr/local/nginx #当前目录下有个configure文件，要执行它。--prefix=的意思是安装到哪个
```



#### **如果出现警告或报错**

提示

```
checking for OS
+ Linux 3.10.0-693.el7.x86_64 x86_64
checking for C compiler ... not found
./configure: error: C compiler cc is not found
```

安装gcc

- yum install -y gcc

```shell
#ubuntu系统的执行下面这个
#因为ubuntu是没用yum的，如果一定要用上面的指令，你就要安装yum后才可运行了（buntu下安装yum我至今没有成功QAQ）。
sudo apt update
sudo apt install build-essential #安装
gcc --version #验证安装成功
```



提示：

```
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```

安装perl库

```
yum install -y pcre pcre-devel
```

```shell
#ubuntu系统的执行下面这个
sudo apt-get update #更新
sudo apt-get install libpcre3 libpcre3-dev #安装
```



提示：

```
./configure: error: the HTTP gzip module requires the zlib library.
You can either disable the module by using --without-http_gzip_module
option, or install the zlib library into the system, or build the zlib library
statically from the source with nginx by using --with-zlib=<path> option.
```

安装zlib库

- yum install -y zlib zlib-devel

```shell
#ubuntu系统的执行下面这个
sudo apt install zlib1g
```



ubuntu安装成功界面如下

![image-20220520090838787](./Nginx.assets\image-20220520090838787.png)



接下来执行

```
make
make install
```



注意：这之后/usr/local/nginx目录下才会有东西



#### **启动Nginx**

进入安装好的目录 /usr/local/nginx/sbin

```
./nginx 启动
./nginx -s stop 快速停止
./nginx -s quit 优雅关闭，完成已经接受的连接请求,才退出
./nginx -s reload 重新加载配置
```



#### **关于防火墙**

##### **关闭防火墙**

​	systemctl stop firewalld.service

```shell
#ubuntu
sudo ufw status #查看防火墙状态，inactive是关闭，active是开启。
sudo ufw enable #开启防火墙。
sudo ufw disable #关闭防火墙。
sudo ufw reload  #重启ufw防火墙
```



##### **禁止防火墙开机启动**

​	systemctl disable firewalld.service

##### **放行端口**

​	firewall-cmd \--zone=public \--add-port=80/tcp \--permanent

##### **重启防火墙**

​	firewall-cmd \--reload

#### **安装成系统服务**

创建服务脚本

​	vi /usr/lib/systemd/system/nginx.service

服务脚本内容

脚本里的路径/usr/local/nginx是按照时路径，根据自己的安装位置修改

```shell
[Unit]
Description=nginx - web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
ExecQuit=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

重新加载系统服务

​	systemctl daemon-reload

启动服务（关闭nginx后再开启）

​	systemctl start nginx.service

##### **开机启动**

​	systemctl enable nginx.service

```
systemctl stop nginx.service  #暂停服务
systemctl status nginx.service #查看状态
systemctl disable nginx.service # 禁止开机启动
```

查看nginx状态

```text
ps -ef|grep nginx
```



#### **容器（Docker)安装**