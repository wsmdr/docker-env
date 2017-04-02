# docker-env

本项目包含
* php-fpm-7.1.3
* nginx-1.10.1
* mysql-5.7
* redis-3.0.7

# 需要的软件

* Git
* Docker 、 Docker-compose


## Docker Install

* [Docker for Mac](https://get.daocloud.io/docker-install/mac)
* [Docker for Windows](https://get.daocloud.io/docker-install/windows)
* Docker for Linux

## 运行步骤

* `cp web/env-project web/.env`
* 在.env文件中 配置
* 进入web目录 执行 `docker-compose up -d` 启动Docker服务
* 浏览器访问 http://127.0.0.1
