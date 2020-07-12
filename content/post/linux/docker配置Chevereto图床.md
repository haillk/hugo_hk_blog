---
title: 'Docker配置Chevereto图床'
tags: 
-
categories: 
- Linux
date: "2020-07-12T13:12:16+08:00"
comment:  true    
---

与你常在

<!--more-->

### 环境配置

debian服务器安装docker-ce和dockers-Compose

##### 安装docker和docker-Compose[官方文档](https://docs.docker.com/engine/install/debian/)

> 包括设置docker储存库
>
> 添加docker官方gpg密钥
>
> 设置存储库

### 新建docker-compose.yaml文件用来构建docker

参考[dockerhub](https://hub.docker.com/r/nmtan/chevereto/)

```yaml
version: '3'

services:
  db:
    image: mariadb
    volumes:
      - database:/var/lib/mysql:rw
    restart: always
    networks:
      - private
    environment:
      MYSQL_ROOT_PASSWORD: chevereto_root
      MYSQL_DATABASE: chevereto
      MYSQL_USER: chevereto
      MYSQL_PASSWORD: chevereto

  chevereto:
    depends_on:
      - db
    image: nmtan/chevereto
    restart: always
    networks:
      - private
    environment:
      CHEVERETO_DB_HOST: db
      CHEVERETO_DB_USERNAME: chevereto
      CHEVERETO_DB_PASSWORD: chevereto
      CHEVERETO_DB_NAME: chevereto
      CHEVERETO_DB_PREFIX: chv_
    volumes:
      - chevereto_images:/var/www/html/images:rw
    ports:
      - 8080:80

networks:
  private:
volumes:
  database:
  chevereto_images:
```

ps:端口设置（主机端口：docker端口）

volumes:图片存储位置

### 启动服务

`docker-compose up -d`