---
title: alpine中的go打包
subtitle:
date: 2024-08-16T22:27:19+08:00
slug: 0b554fe
draft: false
author:
  name:
  link:
  email:
  avatar:
description: go正常打包的二进制在alpine环境中不能正常执行的解决方案
keywords:
license:
comment: false
weight: 0
tags:
  - docker
  - podman
categories:
  - docker
  - podman
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRss: false
hiddenFromRelated: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: false
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->
# go关于alpine打包问题
  因为alpine使用的是musl libc而平时的ubuntu之类的Linux用的glic导致在ubuntu打包的go二进制文件在alpine中无法正常的执行
- 解决方案一
   使用dockerfile的多阶段构建镜像，找一个官方的go的alpine镜像环境在里面打包
   然后再将包放入alpine镜像，并且加入环境变量
   ```yml
  FROM golang:1.23.0-alpine3.20 as builder
  WORKDIR /app
  COPY ./ /app
  RUN ./build.sh linux

  FROM alpine
  WORKDIR /app
  COPY --from=builder /app/bin/redisCmd /usr/local/bin
   ```