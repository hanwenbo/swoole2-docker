# swoole-docker
仅为自己常用的swoole配套工具包，专门swoole 2.x版本出的，请别用错了，fashop的swoole版本支持1.x和2.x


# 启动
> 建议用一个镜像启动两个容器：测试和部署
### 拉取镜像
`docker pull ezkuangren/swoole2`

#### 测试环境方便在命令行里操作：
```sh
docker run -it -p 本地端口:容器端口 -v /你的本地目录:/var/www/project --privileged=true ezkuangren/swoole2 /bin/bash
```
此时会出现一个容器id。

你成功的创建了一个镜像，下次再启动不需要加配置项了（端口，目录...），镜像好比是linux系统，容器是你要装的服务器，装好了之后下次启动不需要再配置各种项了。

/bin/bash 是通过命令行进入到容器，就可以在容器里启动、停止、重启swoole项目了

相关操作：

`docker start 容器id` 启动

`docker stop 容器id`  关闭

`docker rm 容器id`  删除，多个用空格间隔

`docker container update 配置 容器id` 修改

`docker rename 原容器名  新容器名` 改名字，方便找，不然默认是随机生成的


有时候，我们创建容器时忘了添加参数 --restart=always ，当 Docker 重启时，容器未能自动启动，
现在要添加该参数怎么办呢，方法有两种：

1、Docker 命令修改

docker container update --restart=always 容器名

2、直接改配置文件

首先停止容器，不然无法修改配置文件

配置文件路径为：/var/lib/docker/containers/容器ID

在该目录下找到一个文件 hostconfig.json ，找到该文件中关键字 RestartPolicy

修改前配置："RestartPolicy":{"Name":"no","MaximumRetryCount":0}

修改后配置："RestartPolicy":{"Name":"always","MaximumRetryCount":0}

最后启动容器。


#### 部署模式：

```sh
docker run -it -p 本地端口:容器端口 -v /你的本地目录:/var/www/project --privileged=true ezkuangren/swoole2 /bin/sh -c "你要执行的start命令"
```
如 FaShop 的启动：（还未上线，仅为演示）

.... /bin/sh -c "composer install --no-dev && composer dump-autoload -o && composer clearcache && php fashop start"

意思是安装composer的依赖包，重新加载（比如命名空间的改变），清理缓存，开启fashop的项目


# 如何部署FaShop服务器端？

- 首先拉取 https://github.com/mojisrc/fashop 该项目代码资源到本地，假设是放在`/你的本地目录`，那么操作如下：

#### 开发模式：

> docker run -it -p 9510:9510 -v /你的本地目录:/var/www/project --privileged=true ezkuangren/swoole2 /bin/bash

进去之后：
- 如果没`composer install`过的项目，请先`composer install`，然后 `php fashop start`
- 然后访问 `127.0.0.1:9510/install` 进行安装

后台控制面板部署：
- 教程暂无，直接去 https://github.com/mojisrc/fashop-admin 拉取吧

#### 部署模式：


> docker run -it -p 9510:9510 -v /你的本地目录:/var/www/project --privileged=true ezkuangren/swoole2 /bin/sh -c "composer install --no-dev && composer dump-autoload -o && composer clearcache && php fashop start"
