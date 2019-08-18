在windows下安装软件，我们只需要有EXE文件，然后双击，下一步直接OK就可以了。但在linux下需要获取软件包并执行命令来安装。

Linux软件包是文件包的一种，文件包还可以是普通的文件包（一般为单文件或目录的压缩包）。

## 软件包介绍

#### 软件包的分类

**Linux下软件包一般分为二进制包和源码包两种形式。**

二进制包里面包括了已经经过编译，可以马上运行的程序。是计算机认识的程序。你只需要下载和解包（安装）它们以后，就马上可以使用。相当于windows下的exe文件

源码包里面包括了程序员写的原始程序代码，需要在自己的计算机上进行编译转化为二进制包，才能被计算机认识。源代码安装分`配置-编译-安装`三步，时间会比较长。

#### 软件包的组成部分

二进制程序，位于 /bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin 等目录中。
库文件，位于 /lib, /usr/lib, /usr/local/lib 等目录中。Linux中库文件以 .so（动态链接库）或 .a（静态链接库）作为文件后缀名。
配置文件，位于 /etc 目录中。
帮助文件：手册, README, INSTALL (/usr/share/doc/)

#### 软件包的命名规范

**二进制包**：`name-version-release.os.arch.{rpm|deb|...}`

name：程序名称
version：程序版本号
release（发行号）：用于标识RPM包本身的发行号，与源程序release号无关
os：即说明二进制包支持的操作系统版本。如el6（即rhel6）、centos6、suse11
arch：主机平台。如i686、x86_64、amd64、ppc、noarch（不依赖平台）
例：bash-4.3.2-5.el6.x86_64.rpm

**源码包**：`name-version.tar.{gz|bz2|...}`

name：程序名称
version：版本号。又分为：major.minor.release
例：bash-4.3.1.tar.gz

注：软件名和软件包名的区别。软件名一般就是程序的名称，而软件包名会包含一些版本号、主机平台等信息

>Linux系统对文件后缀名不敏感，后缀名只是方便人来识别的。要想知道文件的具体类型可以用file命令来查看`file 文件名`


#### 获取并安装软件包

每个LINUX的发行版都会维护一个自己的软件仓库，我们常用的几乎所有软件都在这里面。这里面的软件绝对安全，而且绝对的能正常安装。通常使用yum或apt即可从这些软件仓库来获取软件并安装。

同时，国内的一些公司也有自己的开源软件仓库，如阿里源、网易源等。因为一些可知的原因，我们可以把默认的源切换国内源。

若这些仓库中没有想要的软件就需要去软件的官网去下载软件包，用到wget工具下载，然后再来安装。若系统中没有wget工具可以执行`{yum|sudo apt} -y install wget`安装

下面会详细讲解这一部分


## 二进制包（binary code）

Linux各发行版本软件包的格式不同，管理软件包的工具也有所不同。

软件包管理工具的作用是提供在操作系统中安装，升级，卸载需要的软件的方法，并提供对系统中所有软件状态信息的查询。

### rpm与yum

Redhat Linux 家族（如CentOS、Opensuse、Fedora）使用`.rpm`二进制软件包格式，并使用rpm及其前端工具（如yum、dnf）为包管理器。

#### rpm

**rpm（RPM Package Manager） 是 Redhat Linux 家族的基础包管理系统**，它用于安装，升级，卸载和提供.rpm包的信息。*缺点是要手动解决依赖关系，较麻烦。*

- 查询所有已经安装的软件包：`rpm -qa`
- 查询指定软件包的内容：`rpm -qip 要查询的软件包`
- 安装软件：`rpm -ivh 要安装的rpm软件包`i：安装，v：校验，h：显示安装进度
- 升级软件：`rpm -Uvh 要升级的rpm软件包`U：升级
- 删除软件：`rpm -e 要删除的rpm软件包`


#### yum

**Yum（ Yellow dog Updater, Modified）是一个rpm包管理系统的shell前端工具**，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包（相当于一次性把所有需要安装的N个rpm包统一下载安装）。它是一个非常受欢迎的、自由而强大的命令行包管理器。

>`yum [options] [command] [package ...]`
options：参数，可选，可组合。
command：要进行的操作。
package操作的对象。

- 安装软件：`yum install -y 要安装的rpm软件包`多个软件包用`逗号`隔开
- 显示所有已经安装和可以安装的程序包：`yum list`
- 检测可升级的包：`yum check-update`
- 更新软件：`yum update`更新全部软件包（也可加要更新的软件包）
- 删除软件：`yum remove 要删除的rpm软件包`
- 清除缓存：`yum clean all`yum仓库若更新，则本地缓存就没有意义了
- 生成缓存：`yum makecache`下载远程yum的文件，在本地生成缓存

**yum客户端安装程序流程**

![yum客户端安装程序流程](http://pcen3n3o2.bkt.clouddn.com/yum%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%AE%89%E8%A3%85%E7%A8%8B%E5%BA%8F%E6%B5%81%E7%A8%8B.jpg)



#### yum 源

yum仓库又称为yum源，是官方维护的软件仓库。yum仓库一般会支持ftp协议，http协议，文件协议。

centOS系统的yum源主配置文件为`/etc/yum.conf`此文件引入了其他配置文件。
源列表文件：`/etc/yum.repos.d/CentOS-Base.repo`，源列表里面都是一些网址信息，每一条网址就是一个源，这个地址指向的数据标识着这台源服务器上有哪些软件可以安装使用。

**epel源**

Redhat家族除了官方维护的软件仓库外，还有一个基于Fedora的一个项目：EPEL (Extra Packages for Enterprise Linux)，为Redhat家族的操作系统提供额外的软件包。
- 安装epel源`sudo yum install -y epel-release`
- 安装完成后更新软件源的缓存`sudo yum makecache`

**国内yum源**

系统默认的yum源是国外的源，易受网速的限制。所以有必要把yum源换为国内的源。国内主要开源的开源镜像站点主要是网易原和阿里源

更换国内yum源的方式：

>进入源文件所在目录`cd /etc/yum.repos.d/`
>备份源文件`mv CentOS-Base.repo CentOS-Base.repo.backup`
>下载aliyun的yum源`wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`
>清除原缓存`yum clean all` 
>生成新缓存`yum makecache`




### dpkg与apt

Debian Linux 家族（如Ubuntu、Debian、Mint）使用`.deb`二进制软件包格式，并使用dpkg及其前端工具（如apt、Aptitude、Synaptic）为包管理器。

**dpkg（Debian PacKaGe） 是 Debian Linux 家族的基础包管理系统**，它用于安装，升级，卸载和提供.deb包的信息。

- 查询所有已经安装的软件包：`dpkg -I`
- 查询指定软件包的内容：`dpkg --info 要查询的软件包`
- 安装软件：`sudo dpkg -i 要安装的deb包`解决依赖`sudo apt-get -f install`
- 升级软件：`sudo dpkg -i 要升级的deb包`
- 删除软件：`dpkg -r 要删除的软件名`（保留其配置信息）`dpkg -P 要删除的软件名` （同时删除配置信息）


**apt(Advanced Packaging Tool）是一个dpkg 包管理系统的前端工具**，能够从指定的服务器自动下载deb包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包（相当于一次性把所有需要安装的N个deb包统一下载安装）。它是一个非常受欢迎的、自由而强大的命令行包管理器。
apt-get

- 安装软件：`sudo apt -y install 要安装的deb包`
- 更新软件列表：`sudo apt update`
- 更新软件：`sudo apt upgrade`
- 删除软件：`sudo apt autoremove 要删除的deb包`

sudo apt update会访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑
sudo apt upgrade会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，就会提示你更新
总而言之，update是更新软件列表，upgrade是更新软件。

注意：如果源里面有系统更新，upgrade会升级系统。可能会造成原系统下命令失效，代码不能正常运行。线上项目一般不会更新系统。

#### apt源

Ubuntu系统的源列表文件为`/etc/apt/sources.list`，源列表里面都是一些网址信息，每一条网址就是一个源，这个地址指向的数据标识着这台源服务器上有哪些软件可以安装使用。

同样，系统默认的apt源是国外的源，所以我们需要将默认的源切换为国内源。
常用的国内源有阿里源（这是中国官方源）、网易源、搜狐源、清华源、中科大源、浙大源等等

更换国内apt源的两种方式：

>**桌面方式**：
应用程序->软件和更新->下载自->其他站点->找到相应源如阿里源->选择服务器->重新载入（系统的工作是更新缓存，写入一个源文件并启用）
**命令行方式**：
进入源文件目录`cd /etc/apt`
备份源列表文件`sudo cp sources.list sources.list.bak`
编辑源文件`sudo vim sources.list`
删除文件内容，找到国内源，复制到此文件中
更新缓存`sudo apt update`


## 源码包（source code）

有时候系统提供的二进制包不符合我们的使用需求，这时候就要自己下载源码包来编译安装。比如自己编译安装LNMP环境。

>下载源码包->解压缩到指定目录->配置->编译->安装

#### 下载源码包

**wget**

wget名称是 World Wide Web 与 get 的结合，它是一个从网络上指定的URL下载文件的工具，支持通过 HTTP、HTTPS、FTP 三个最常见的 TCP/IP协议下载，并可以使用 HTTP 代理。支持在用户退出系统的之后在继续后台执行，直到下载任务完成。支持递归下载。若系统中没有wget工具可以执行`{yum|sudo apt} -y install wget`安装

语法：`wget[选项][参数]`
- `wget url网址`下载单文件
- `wget -O 保存的文件名 url网址`重命名
- `wget -P url网址`下载到指定目录
- `wget -c url网址`断点续传
- `wget -b url网址`后台下载

**lrzsz**

rz，sz是Linux/Unix同Windows进行ZModem文件传输（上传下载）的命令行工具

安装lrzsz工具包`yum  install lrzsz -y`

上传命令：`rz`。为避免文件太大或乱码问题使用`rz -be`来上传
下载命令：`sz 要下载的文件名`
>注：普通用户上传下载若失败很有可能是权限不够，命令前加`sudo`即可
#### 解压缩

gzip、bzip是压缩工具。tar是一个打包工具。
 
#### gzip

- 压缩文件`gzip 文件名`结果：`文件名.gz`解压缩`gzip -d 文件名.gz`
- 压缩目录 `gzip -r 目录名`解压缩`gzip -dr 目录名`
>注意：压缩文件时同时删除原文件。压缩目录时实际不压缩目录，而是把目录下的每一个文件压缩成gz格式

#### bzip

跟gzip用法一样，区别是底层算法不同，压缩大小不同，压缩格式不同，而且bzip不能压缩目录

#### tar
tar是一个打包工具，本身自带了压缩的功能

打包目录的方式

##### 先打包目录再压缩

1. `tar cvf 打包后的文件名.tar 要压缩的目录名`c参数：create v参数：显示过程 f参数：跟要打包的目录
2. 压缩文件`gzip 打包后的文件名.tar`

##### 打包及压缩（常用）

打包并通过gizp方式压缩
`tar czvf 压缩后的文件名.tar.gz 要压缩的目录名`z参数：指定gizp方式压缩（tar新版本不需要加？）
解压缩`tar xzvf 压缩后的文件名.tar.gz`x参数：解压缩
解压缩到指定目录`tar xzvf 压缩后的文件名.tar.gz -C 目标目录`C参数：指定目录

打包并通过bzip2压缩
`tar cjvf 压缩后的文件名.tar.bz2 要压缩的目录名`j参数：指定bizp2方式压缩
解压缩`tar xjvf test.tar.bz2`
解压缩到指定目录`tar xjvf 压缩后的文件名.tar.bz2 -C 目标目录`C参数：指定目录
>实际场景中直接用tar命令来打包并压缩目录（z或j参数指定压缩的方式）
>gizp命令用来把目录下的所有文件一个个压缩。
>压缩文件用gizp或bizp2命令即可
>压缩解压缩不仅可以用于软件包，普通文件包同样适用

#### 配置、编译、安装

这部分内容比较多，详见后续文章