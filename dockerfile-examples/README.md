[TOC]

# Dockerfile Examples



## 前言

参考下面的参考文档，整理一些常用的基础镜像、Dockerfile，以供自己编写Dockerfile时参考，并加深对Docker镜像原理的认识。



## 参考文档



* https://docs.docker.com/engine/reference/builder/
* https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
* [如何Docker化任意一个应用？你需要参照这10步](https://mp.weixin.qq.com/s?__biz=MzIzNjUxMzk2NQ==&mid=2247489522&idx=1&sn=7328aac4967fafa967d67cc79f123353&chksm=e8d7e830dfa06126cf2f12e12495948553c001c626bb4809d0b19bfeeb9aed091a972a525566&mpshare=1&scene=1&srcid=0708W9rNsIQAMN7lSRfVAmOr&pass_ticket=z2rJIQxFZ8g5%2B8EtzowJFNB4G9XdRV1CRytAT0MdZckD1QGKWMU%2FDZFeEIyekf%2FQ#rd)
* [5个构建第一个Java镜像的小窍门](http://dockone.io/article/2039)





## 镜像官网



Docker镜像官网（Docker Hub）: https://hub.docker.com

Google镜像官网（gcr.io）：

阿里云镜像官网：



## 选择基础镜像



因为Docker镜像总是基于基础镜像来构建的，因此选择的基础镜像越高级，我们要做的底层工作就越少。



比如，如果构建一个Java应用的镜像，选择一个openjdk的镜像作为基础镜像比选择一个alpine镜像作为基础镜像要简单地多。



### 操作系统基础镜像



| 镜像名称 | 大小   | 用处                                                         |
| -------- | ------ | ------------------------------------------------------------ |
| busybox  | 1.15MB | 临时测试用                                                   |
| alpine   | 4.41MB | 主要用于测试，也可用于生产环境                               |
| centos   | 200MB  | 主要用于生产环境，支持CentOS/Redhat，常用于追求稳定性的企业应用 |
| ubuntu   | 81.1MB | 主要用于生产环境，常用于人工智能计算和企业应用               |
| debian   | 101MB  | 主要用于生产环境                                             |



**busybox**

描述：可以将busybox理解为一个超级简化版嵌入式Linux系统。

官网：https://www.busybox.net/

镜像：https://hub.docker.com/_/busybox/

包管理命令：apk, lbu

包管理文档：https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management





**Alpine**

描述：Alpine是一个面向安全的、轻量级的Linux系统，基于musl libc和busybox。

官网：https://www.alpinelinux.org/

镜像：https://hub.docker.com/_/alpine/

包管理命令：apk, lbu

包管理文档：https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management



**CentOS**

描述：可以理解CentOS是RedHat的社区版

官网：https://www.centos.org/

镜像：https://hub.docker.com/_/centos/

包管理命令：yum, rpm



**Ubuntu**

描述：另一个非常出色的Linux发行版

官网：http://www.ubuntu.com/

镜像：https://hub.docker.com/_/ubuntu/

包管理命令：apt-get, dpkg



**Debian**

描述：另一个非常出色的Linux发行版

官网：https://www.debian.org/

镜像：https://hub.docker.com/_/debian/

包管理命令：apt-get, dpkg



### 编程语言基础镜像



**Java基础镜像**

* https://hub.docker.com/_/java/ （Deprecated)
* https://hub.docker.com/_/openjdk/



> 由于Oracle JDK license问题，Docker官方的Java基础镜像使用的是OpenJDK而不是Oracle JDK。



**Python基础镜像**

- https://hub.docker.com/_/python/



**NodeJs基础镜像**

* https://hub.docker.com/_/node/

  

### 应用基础镜像



**Nginx基础镜像**

* https://hub.docker.com/_/nginx/



**Tomcat基础镜像**

* https://hub.docker.com/_/tomcat/



**Jetty基础镜像**

* https://hub.docker.com/_/jetty/



### 其它基础镜像例子



**Maven基础镜像**

* https://hub.docker.com/_/maven/



**Jenkins基础镜像**

* https://hub.docker.com/r/jenkins/jenkins/



**GitLab基础镜像**

* https://hub.docker.com/r/gitlab/gitlab-ce/



## 最佳实践



* 不要将任何持久化数据保存在容器内

* 保持构建上下文目录只包含Dockerfile和需要用到的文件

* 不要安装不需要用到的包

* 一个镜像只做一件事情

* 最小化层的数目（尽量合并`RUN`, `COPY` 和 `ADD`  命令）

* 有些命令必须合并成一个命令，避免分层后不能读取的问题（比如`apt-get update`和`apt-get install` 要写在同一行）

* 基本顺序：

  1) 安装需要构建应用的工具

  2) 安装或更新库文件依赖

  3) 构建出应用

* 不建议以root用户运行容器

* 在代码仓库中修正文件属性，而不是在Dockerfile中



参考文档： https://docs.docker.com/develop/develop-images/dockerfile_best-practices/



## FAQ



Q: `ARG` 和`ENV` 命令的区别。

A:  `ARG` 支持在构建镜像时（build-time）修改参数的值，比如`docker build --build-arg var=xxx`。

`ENV` 支持在运行容器时(run-time) 修改参数的值，比如`docker run -e var=yyy` 。

* https://stackoverflow.com/questions/41916386/arg-or-env-which-one-to-use-in-this-case

* https://docs.docker.com/engine/reference/builder/#arg



Q: `ADD` 和 `COPY` 命令的区别。

A: 推荐使用COPY命令。

* https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#add-or-copy



Q: Docker build的cache问题。

A: `ADD` 和`COPY` 命令利用cksum来检查同名文件是否存在在缓存中，但是对cksum的计算不包括last-modified time和last-accessed time，有可能导致缓存机制失效。如果你的应用每次都基于一个同名文件，需要注意这个问题。

https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache





Q: `RUN` , `CMD`  和 ``ENTRYPOINT`的区别。

A:  `RUN` 命令是在`docker build` 构建时被执行，在容器运行时不会被执行。

`CMD` 和 `ENTRYPOINT` 命令是在`docker run` 运行容器时被执行，在容器构建时不会被执行。

Docker缺省的ENTRYPOINT是`/bin/sh -c`。

* https://docs.docker.com/engine/reference/builder/#entrypoint

* https://docs.docker.com/engine/reference/builder/#cmd

* https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact

* https://aboullaite.me/dockerfile-run-vs-cmd-vs-entrypoint/

  