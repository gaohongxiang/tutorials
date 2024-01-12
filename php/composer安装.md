>Composer 是 PHP 的一个包管理工具

composer能解决的问题

- 一个项目依赖于若干个库
- 其中一些库依赖于其他库
- 声明所依赖的东西

Composer会找出需要安装的包，并安装它们（将它们下载到项目中）


## Linux下的安装

ubuntu系统：`sudo apt install -y composer`

centOS系统：`sudo yum install -y composer`

或者
执行如下命令(安装composer)：
`curl -sS https://getcomposer.org/installer | php`

注意： 如果上述方法由于某些原因失败了，你还可以通过 php >下载安装器：
`php -r "readfile('https://getcomposer.org/installer');" | php`

可以通过 --install-dir 选项指定 Composer 的安装目录
`curl -sS https://getcomposer.org/installer | php -- --install-dir=/home`

2、可以执行如下命令让 composer 在你的系统中进行全局调用：
`mv composer.phar /usr/local/bin/composer`

3、验证安装是否成功，执行如下命令
`[root@localhost]#  composer`

4、之后可以在任意文件下建一个composer.json，并填写如下命令：
`{"require":{}}`

然后可以在该文件夹下运行composer的相关命令了，

如：`composer  install;`    `composer  update;`

#### 更换国内composer源

##### 将全部的镜像修改为国内镜像

`composer config -g repo.packagist composer https://packagist.phpcomposer.com`

##### 将当前项目的composer修改为国内镜像

`composer config repo.packagist composer https://packagist.phpcomposer.com`

##### 更换composer国内镜像源的时候的错误处理

Q：错误信息如下

touch(): Unable to create file /home/ghx/.composer/config.json because Permission denied

Could not read /home/ghx/.composer/config.json                               
                                                                               
file_get_contents(/home/ghx/.composer/config.json): failed to open stream:Permission denied   
  
A：首先猜测是权限不够，所以把`composer config -g repo.packagist composer https://packagist.phpcomposer.com`前加`sudo`。仍然不行，
提示：Do not run Composer as root/super user! See https://getcomposer.org/root for details。意思是不让用root权限

再一看，`could not read` 意思是不能读，那么应该是文件权限的问题

>方法1：哪里无权限就修改哪里

修改config.json文件的权限`sud chmod 777 /home/ghx/.composerconfig.json`
再次运行提示auth.json 不能读，修改权限`sud chmod 444 /home/ghx/.composerauth.json`。
问题解决。

根据提示给相应的文件添加相应权限即可

>方法2：直接递归修改composer根目录的权限
```
sudo chmod -R 777 /home/ghx/.composer
```

##### composer install 错误处理 

1、Your requirements could not be resolved to an installable set of packages. 这是因为不匹配composer.json要求的版本。

```
composer install --ignore-platform-reqs
``` 

忽略安装，当composer.json版本较高时,就用它!

2、
Cannot create cache directory /home/ghx/.composer/cache/repo/http---packagist.phpcomposer.com/, or directory is not writable. Proceeding without cache
Cannot create cache directory /home/ghx/.composer/cache/files/, or directory is not writable. Proceeding without cache

如果出现类似的问题，就是composer目录下的某个目录或文件权限不够。需要修改权限。所以，可以直接把composer目录及以下目录修改成最高权限。

```
sudo chmod -R 777 /home/ghx/.composer
```
如果更换镜像源的时候已经修改了此目录最高权限，则不会提示此错误了

3、vendor does not exist and could not be created. 


加sudo 权限`sudo composer config -g repo.packagist composer https://packagist.phpcomposer.com`

[慎用composer update](https://blog.csdn.net/wulove52/article/details/78392663)

## window下的安装

1. 下载composer.exe 
2. 下一步下一步直到结束
3. 将PHP的路径添加到系统环境变量
4. 将composer镜像调整为国内镜像

#### 更换国内composer源

命令同linux下命令


#### 安装可能出现的问题及解决方法

Q：找不到模块
A：修改`php.ini`文件的扩展路径
`extension_dir = "D:/FullStack/wamp/php7/ext"`

Q：缺少`feclient.dll`文件
A：C:\Windows\SysWOW64目录下添加`feclient.dll`文件（网上下的）

linux下



### 其他注意项（`php.ini`文件）
- asp_tag 扩展要关闭
- openSSL 扩展要开启
- pdo_mysql 扩展要开启
- extension=pdo_firebird 扩展要开启
- extension=mbstring  扩展要开启
- extension=interbase  扩展要开启