---
title: Learn-Docker
date: 2019-07-25 18:41:17
categories:
- [Docker]
tags:
- [Docker]
---
<p>最近在学Docker,用的是国内的《Docker 容器与容器云》，这里记录一下我遇到的问题。</p>
<p>第一个Docker应用栈，HAProxy作为代理节点，Django 作两个web应用节点，redis作一个主数据库节点，另外两个数据库作从数>据库节点。</p>
<p>1. docker pull把镜像拉下来启动时，书上提到了第一次启动用</p>

        docker run -it --name redis-master redis /bin/bash

<p>但没有提到启动后如何后台运行，可能已激动就exit了，后面两个从数据库link时就会发现报错，因为前面前面启动的容器已经在exit时停止运行了，所以连接不上。那怎么办呢？</p>

        docker exec -it redis-master /bin/bash

<p>以这种方式启动，即使exit容器也不会停止。</p>
<p>2. 接下来是找redis映射到宿主机的目录，书上用的是 docker inspect --format "{ .Volume }" bc8e</pre> 执行后你会发现，找不到，把Volume改为Mounts试试。目录也不是/var/lib/docker/vfs/......而是/var/lib/docker/volumes/.....</p>
<p>3. 然后是redis配置文件，我是直接下载 [redis](http://download.redis.io/redis-stable/redis.conf "redis.conf")官方配>置文件进行修改的，但在进行数据库节点测试时发现，数据始终同步不到从节点，slaveof master 6379确是返回ok的。最后>查得，要在**redis.conf**文件里在*bind 127.0.0.1后面加上从节点的IP。 因为127.0.0.1是只允许本机访问，比如我是这样写的。</p>

        bind 127.0.0.1 172.17.0.3 172.17.0.4 172.17.0.2
<p>这是主节点的，写上自己的IP和两个从节点的IP，如果是从节点就写上自己IP和主节点IP，有些人用bind 0.0.0.0也可以，但这样可能会不安全。接下来重启一下redis-server，再测试应该就好了。</p>
