---
title: "[docker]常用docker指令筆記整理"
date: 2020-06-22T14:33:48+08:00
draft: false
categories:
  - Tech筆記
tags: ["docker"]

---


本章紀錄
- 常用docker指令筆記整理


<!--more-->

#### 啟動 docker image
使用docker有兩種方式，docker hub 有些會寫好內容 ex:[mongo-docker hub](https://hub.docker.com/_/mongo "mongo-docker hub")

- 一種是直接下docker run，再下對應參數，下一次起一個程式。
	ex:docker run -v D:\home/work:/home/work --name myub -it ubuntu bash
	
|   |   |
| ------------ | ------------ |
|  --name | 替container取名  |
|  -p（小寫） |  hostPort對應containerPort |
| -v  |  分享空間 |
| -it  |  代表在執行Docker 虛擬容器環境時，開啟虛擬終端機，以互動的模式執行 |
| --cpus=1.5  |  限制 CPU 使用量 |
| --memory=300m --memory-swap=1g  |  限制記憶體與 swap 交換空間的用量 |

- 另一種是把它寫成docker-compose.yml方式，可以一次啟動多個程式。
有些image會提供已編寫好的內容。

|   |   |
| ------------ | ------------ |
|  docker-compose up -d |  啟動，-d detached 在背景執行 |
| docker-compose stop | 停止容器  |
| docker-compose down | 刪除容器  |



#### 其他常用指令
|   |   |
| ------------ | ------------ |
| $docker --version<br>Docker version 18.09.2, build 6247962  | 查看簡易docker版本  |
|  docker ps |  查看正在啟動的CONTAINER與ID  |
|  docker stop 'CONTAINER_ID' | 停止 Docker 容器   |
|  docker kill 'CONTAINER_ID' |  強制停止 Docker 容器  |
|  docker restart 'CONTAINER_ID'|  重新啟動 Docker 容器  |
|  docker ps -a |   -a :顯示所有的容器，包括未運行的 |
|  docker search 'ubuntu' |  docker search 'xxx' 查詢可下載 image  |
| docker pull 'ubuntu'   |  docker pull 'xxx' 下載 image  |
| docker images  | 查看下載image與ID   |
| docker system df  | 查看使用的磁盘空间 |
| docker stats  | 查看容器使用的系统资源 每隔 1 秒刷新 |
| docker stats --no-stream  | 查看容器當前的系統資源 |
| docker stats 'CONTAINER ID or name'... | 指定查看特定容器 |
| docker cp 'CONTAINER ID':/xx local <br> Ex: docker cp 9e701a5209fe:/data/db C:/test_temp| 複製CONTAINER內資料出來本地 |
| docker save  | 備份 Docker Image |

清理指令系列另外寫:
因為image都有一定的大小，使用完不用要記得清除。

|   |   |
| ------------ | ------------ |
| docker rm 'CONTAINER_ID'  | 刪除 container  |
| docker rmi  'image_ID'| 刪除 image  |
| docker rm $(docker ps -a -q) | 刪除所有容器(container)//在powershell下才有用 |
|  docker stop $(docker ps -a -q) | 停止所有容器 //在powershell下才有用   |


  

------------


#### 參考文章

1.[Docker 常用指令與容器操作教學](https://blog.gtwang.org/linux/docker-commands-and-container-management-tutorial/ "Docker 常用指令與容器操作教學") <br>
2.[谁用光了磁盘？Docker System命令详解](https://blog.fundebug.com/2017/04/19/docker-system-explain/ "谁用光了磁盘？Docker System命令详解")
3.[清理Docker，删除没用的文件]( https://www.fengzifz.com/2017/03/27/clean-docker/ "清理Docker，删除没用的文件")
4.[查看 docker 容器使用的资源](https://www.cnblogs.com/sparkdev/p/7821376.html "查看 docker 容器使用的资源")

------------

