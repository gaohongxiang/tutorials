项目地址：[gaohongxiang/docker-lnmp](https://github.com/gaohongxiang/docker-lnmp)


## 注意

不要把 docker 当做数据容器来使用，数据一定要用 volumes 放在容器外面

不要把 docker-compose 文件暴露给别人， 因为上面有你的服务器信息

多用 docker-compose 的命令去操作， 不要用 docker 手动命令&docker-compose 去同时操作

写一个脚本类的东西，自动备份docker 映射出来的数据。

不要把所有服务都放在一个 docker 容器里面

## 参考
[twang2218/lnmp-nginx(docker hub)](https://hub.docker.com/r/twang2218/lnmp-nginx/)
[twang2218/docker-lnmp(github)](https://github.com/twang2218/docker-lnmp)

[Docker笔记一：基于Docker容器构建并运行 nginx + php + mysql ( mariadb ) 服务环境](https://blog.csdn.net/jkx1132/article/details/54023391)

[用Docker定制你的Golang开发环境](https://juejin.im/entry/59bb70e5f265da065352c83f)

代码挂载问题：[请教下代码放在Docker里面还是外面呢](http://dockone.io/question/24)
数据挂载问题：