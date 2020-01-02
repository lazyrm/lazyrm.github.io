---
layout: post
title:  "使用Docker 之我本机常用的测试环境!"
date:   2019-12-03 16:21:00 +0800
categories: Docker
tag: env,contain
---

* content
{:toc}

## 背景

1. 开发人员需要很大一部分的开发环境都是需要用，但是又不经常用，比如再某个项目中需要用到mq、redis或者需要需要在不同的数据下面表现情况
2. 大概是给电脑减轻负重

---

## 常用的镜像及启动方法

1. microsoft/mssql-server-linux
   微软提供的镜像，解决本地需要安装sql_server如此之大的数据库的麻烦，至于客户端可以用微软的ssms或者第三方的navcat都行，不过建议牵着，因为真的很搭

    ```shell
    docker run --name MSSQL_1434  --restart=always
    -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=p@ssword" -p 1434:1433
    -v dockerfile/mssql:/var/opt/mssql -d microsoft/mssql-server-linux:tag
    ```

2. rabbitmq:management
   rabbitmq消息队列
   这个是docker提供的官方镜像了，tag有用到management是因为需要管理后台

    ```shell
    docker run -d --hostname my-rabbit --name rabbit -e RABBITMQ_DEFAULT_USER=admin
    -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 -p 25672:25672 -p 61613:61613 -p 1883:1883 rabbitmq:management
    ```

    ```shell
    docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq rabbitmq:management
    ```

3. redis:latest
    redis高速缓存
    也是官方提供的镜像了，就用最新版本好了

    ```shell
    docker run --name dockerredis -p 6379:6379 -d --restart=always redis:latest
    --requirepass "p@ssword" redis-server --appendonly yes
    ```

    ```shell
    docker run -itd --name dockerredis -p 6379:6379 redis
    ```

4. donilan/mysql-utf8mb4
   为什么不用官方的镜像，这个是在我刚刚在接触docker使用的时候用的第二个镜像，为什么不是第一个，我想第一个都用来run helloworld了吧，其实用docker多了之后自己也能做mb4的镜像，只不过看了一下这个人的源码发现也没啥不友好的东西，就继续使用了，也是因为仅仅是测试环境所以保留着，想用官方的镜像直接pull docker.io/mysql 这个就好了

   ```shell
    docker run --name mysql_utf8mb4 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=p@ssword
    -v dockerfile/mysql/data:/var/lib/mysql -d donilan/mysql-utf8mb4
   ```

   ```shell 给懒惰的你 哈哈
    docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=p@ssword
    -v dockerfile/mysql/data:/var/lib/mysql -d docker.io/mysql

   ```

5. nginx
   frp的时候方便做转发

   ```shell
   docker run -p 80:80 --name five-nginx -p 443:443 -v /data/nginx/cert:/etc/nginx/cert -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/log:/var/log/nginx -d nginx
   ```

      ```shell
   docker run -p 80:80 --name five-nginx -p 443:443 -v C:\Users\lazyrm\Documents\99dockerfile\nginx\cert:/etc/nginx/cert -v C:\Users\lazyrm\Documents\99dockerfile\nginx\conf\nginx.conf:/etc/nginx/nginx.conf -v C:\Users\lazyrm\Documents\99dockerfile\nginx\html:/usr/share/nginx/html -v C:\Users\lazyrm\Documents\99dockerfile\nginx\log:/var/log/nginx -d nginx
   ```

---
持续更新

---

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
