---
title: "[Docker]快速啟動可用VNC進去連線之ubuntu"
date: 2020-06-22T15:24:14+08:00
draft: false
categories:
  - Tech筆記
tags: ["docker"]
---



### 用docker啟動ubuntu，並用VNC進去連線

> 一般來說docker可以快速去建立一個ubuntu環境，但如果習慣畫面操作，還需要再安裝VNC設定，不知道有沒有人做好的VNC版本呢? 網路上找到一個docker image專案可以做到這件事->[docker-ubuntu-vnc-desktop](https://github.com/fcwu/docker-ubuntu-vnc-desktop "docker-ubuntu-vnc-desktop")

<!--more-->

### 遵照git readme指示啟動:

#### 網頁版本連線
Quick Start <br>
Run the docker container and access with port 6080<br>

docker run -p 6080:80 -v /dev/shm:/dev/shm dorowu/ubuntu-desktop-lxde-vnc
Browse http://127.0.0.1:6080/<br>

### VNC 版本連線
docker run -p 6080:80 -p 5900:5900 -v /dev/shm:/dev/shm dorowu/ubuntu-desktop-lxde-vnc<br>

下載VNC工具[realvnc](https://www.realvnc.com/en/connect/download/viewer/ "realvnc") 啟動之後連VNC(local:127.0.0.0)就可以一樣看到畫面了<br>