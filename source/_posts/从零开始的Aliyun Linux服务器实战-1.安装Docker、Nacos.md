---
title: 从零开始的Aliyun Linux服务器实战-1.安装Docker、nacos
comments: true
categories: 
  - [实战]
tags: 
  - Nacos
  - Docker
  - Aliyun 服务器
updated: 2023-12-26 19:19:38
cover: https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/202312261919952.png
---

### 安装Docker

官方首推的安装方式是安装Docker Desktop，但是我们只希望安装 Docker Engine， Docker Engine 和 Docker Desktop区别见。[FAQs for Docker Desktop for Linux | Docker Docs --- 适用于 Linux 的 Docker Desktop 常见问题解答 |Docker 文档](https://docs.docker.com/desktop/faqs/linuxfaqs/#what-is-the-difference-between-docker-desktop-for-linux-and-docker-engine)

我们通过 Linux 的包管理工具安装，这里笔者使用的系统是 Alibaba Cloud Linux 3.2104 LTS 64位，查阅和发现建议使用新一代的rpm软件包管理器安装社区版Docker(docker-ce)^1^，通过工具安装即可

```shell
# 安装 dnf
yum -y install dnf
# 设置 docker-ce dnf 源
dnf config-manager --add-repo=https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 执行如下命令，安装Alibaba Cloud Linux 3专用的dnf源兼容插件
dnf -y install dnf-plugin-releasever-adapter --repo alinux3-plus
# 安装 docker-ce 
dnf -y install docker-ce --nobest

# 查看是否成功
dnf list docker-ce

```

==！注意！：安装 docker-ce部分==资料给的是 `dnf -y install docker-ce --nobest`,选择最适合的版本，笔者尝试后发现无法正常启动，缺失了部分组件，应该使用 `dnf -y install docker-ce --allowerasing ` 强制升级后可以运行

[^1]: [部署并使用Docker（Alibaba Cloud Linux 3）-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/1328603)



### 启动Docker

```shell
# 启动 docker 服务
systemctl start docker
# 查看 Docker 服务的运行状态
systemctl status docker
```



### 安装 Nacos

> [Nacos 快速开始-官方文档](https://nacos.io/zh-cn/docs/quick-start.html)

笔者安装 Nacos 主要用于项目开发，希望能够可视化地查看数据的存储情况，希望给 Nacos 使用外置数据库，即MySQL。根据 Nacos 手册，需要安装MySQL 5.6.5+版本，并初始化nacos的数据库和表，然后在nacos的配置文件中添加MySQL的数据源信息，这里我们可以使用 `docker-compose.yml` 来生成，方便管理划分

要运行这个文件，我们还需要 [**docker compose** ](https://docs.docker.com/compose/)工具，继续使用 dnf 安装

```shell
dnf -y install docker-compose-plugin
# 查看是否安装成功
docker compose version
```

这里使用官方手册中提供的，种类多还好用，可以自由diy

首先我们需要安装 `git`

```shell
sudo dnf -y install git-all
```

克隆官方项目

```shell
git clone https://github.com/nacos-group/nacos-docker.git
cd nacos-docker
```

选择使用 MYSQL8 对应的配置文件安装

```shell
docker compose -f example/standalone-mysql-8.yaml up
```

所需镜像可以手动拉取，也可以由 docker-compose 从仓库中拉取

在 `docker-compose.yml` 可用指令

```shell
#：创建并启动你的应用程序
docker-compose up

#：停止并删除你的应用程序
docker-compose down

#：查看你的应用程序的状态
docker-compose ps

#：查看你的应用程序的日志
docker-compose logs

#：构建或重新构建你的应用程序的服务
docker-compose build

#：拉取你的应用程序的服务的镜像
docker-compose pull

#：推送你的应用程序的服务的镜像
docker-compose push

#：在你的应用程序的服务上运行一个一次性的命令
docker-compose run

#：在你的应用程序的服务上执行一个交互式的命令
docker-compose exec

#：验证并查看你的 docker-compose.yml 文件的内容
docker-compose config
```

  提示运行成功就可以访问啦！

> 记得把服务器对应的防火墙端口打开，否则访问不到