>Linux下，同一个软件，会把可执行程序，库，文档安装到不同的目录里，不会只放到一个目录下。

## 安装软件

#### 1、yum|apt安装二进制软件

centOS：`yum install -y 要安装的rpm软件包名`

ubuntu：`apt-get install -y 要安装的deb软件包名`

yum|apt安装的二进制软件是事先已经编译好的，存放在系统线上仓库中，由系统统一管理。其优点是安装使用容易，缺点则是缺乏灵活性，如果该软件包是为特定的硬件/操作系统平台编译的，那它就不能在另外的平台或环境下正确执行。还有可能系统仓库中的软件版本不是我们想要的软件版本。

yum|apt安装的软件的文件位置是系统指定的，各种文件可能分布于整个文件系统的任何角落。

软件安装后的文件（含配置文件）
软件依赖的包
软件全局环境变量文件（此文件是自己配置的）

#### 2、编译安装源码文件

>获取源码包->解压缩到指定目录->创建守护进程账户->解决依赖关系->配置majefile文件->编译并安装->清除编译过程中产生的临时文件(`make clean`)->配置（环境变量、开机自启动、软件服务相关配置）

编译安装源码文件顾名思义需要用户自己编译成可执行的二进制代码并进行安装，其优点是配置灵活，可以随意去掉或保留某些功能/模块，适应多种硬件/操作系统平台及编译环境，缺点是难度较大。

[Linux 编译安装 Mysql-5.7](https://broqiang.com/posts/11)
[Linux 编译安装 PHP-7.1.16](https://broqiang.com/tutorials/linux-basic-tutorial/59/compile-and-install-php)
[Linux 编译安装 Nginx-1.14.0](https://broqiang.com/tutorials/linux-basic-tutorial/60/compile-and-install-nginx)

编译安装的文件所在的位置都是自己指定的

软件包压缩文件
软件包解压缩文件
软件安装后的文件（含配置文件）
软件依赖的包（系统指定）
软件全局环境变量文件


## 卸载软件

>卸载无非就是卸载安装的软件、依赖的软件、与软件相关的文件

### 卸载yum|apt安装的软件

#### 查询相关信息

Linux下，同一个软件，会把可执行程序，库，文档安装到不同的目录里，不会只放到一个目录下。

##### centOS

查询当前系统中安装的所有的软件包`rpm -qa`

查询指定的软件包`rpm -qa | grep -i 软件包名`

查询指定软件包的安装位置`rpm -ql 软件包名`

查询与指定软件相关文件位置`whereis 指定软件`或`sudo find / -name 指定软件`

##### ubuntu

查询所有已经安装的软件包：`dpkg -I`

查询指定软件包的内容：`dpkg --info 要查询的软件包`

查询与指定软件相关文件位置`whereis 指定软件`或`sudo find / -name 指定软件`

#### 卸载

##### centOS

`rpm -e 要卸载的软件名 --nodeps`

`yum remove 要卸载的软件名`

>安装的时候是安装软件包名，卸载是卸载软件名

全局查找与指定软件相关的目录或文件
`whereis 指定软件`或`sudo  find  /  -name  指定软件`
删除查到的文件`sudo rm -rf 目录或文件`

[CentOS下如何完全干净卸载MySQL](https://www.linuxidc.com/Linux/2016-12/137941.htm)

##### ubuntu

`dpkg -r 要卸载的软件名`（保留其配置信息）
`dpkg -P 要卸载的软件名` （同时删除配置信息）

`sudo apt-get autoremove 要卸载的软件名`

查询出与指定软件相关的软件
`dpkg --get-selections|grep 指定软件`
一一删除，还是上面删除命令。

全局查找与指定软件相关的目录或文件
`whereis 指定软件`或`sudo  find  /  -name  指定软件`
删除查到的文件`sudo rm -rf 目录或文件`

[Ubuntu彻底删除nginx](https://www.jianshu.com/p/8f8c6c098c4b)

##### ubuntu下删除已经删除的软件包的残留配置文件

`dpkg -l |grep "^rc"|awk '{print $2}' |xargs apt -y purge`
如果命令中apt不行就换成aptitude

`dpkg -l`列出系统中安装的所有包的状态。`ii`开头的是正常安装的包，`rc`开头的则是删除但仍留下配置文件的包，其他状态则是有错误的状态

`grep "^rc"`提取以 `rc`开头的包，即被删除但仍残留配置文件的包的信息的行。

`awk '{print $2}'`打印这些包的名字

`xargs apt -y purge`把上述输出的要清除配置文件的包的名字放在 apt -y purge 后面，purge命令会清除配置文件

### 卸载编译安装的软件

各文件位置都是在编译时指定的，找到这些目录及文件删除即可