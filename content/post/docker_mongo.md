---
title: "[ docker ]啟動 mongo DB 與簡單操作"
date: 2020-05-01T10:30:25+08:00
draft: false
cover: /img/docker.jpg
summary: "Start mongo in docker and mongo tool."
categories:
  - Tech筆記
tags: ["mongo","docker"]

---

本章介紹
- 使用docker-compose快速啟動mongodb
- 簡單的工具操作與MongoDB Shell 更新資料

<!--more-->

> MongoDB是NoSQL的資料庫，以文件儲存資料，一般資料庫開Table須定義欄位(大小、型別、名稱等)，但是Collection完全不事先定義欄位，每筆document可以有不等數量的欄位。。

名詞對應：

|  MongoDB | RDBMS  |  意思 |
| ------------ | ------------ | ------------ |
|  db |   DB|   資料庫|
|  collection | Table  |表格   |
| document  |  record | 一筆紀錄  |

# 1.啟動mongo docker

這邊直接用編輯docker-compose.yml  貼上網路找到的內容

```yaml
version: '3.1'

services:

  mongo:
    image: mongo
    ports:
      - "27017:27017"
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
```

啟動
```bash
➜  mongo git:(master) ✗ docker-compose up -d   
Creating network "mongo_default" with the default driver
Creating mongo_mongo_1         ... done
```


# 2.操作資料內容
以下可用透過一些網上mongo工具 ex Robo3T操作

先進行連線進入介面host: localhost, port: 27017

CreateDB"test"，新增一筆Collection"user"，與新增Documanet如下(Json格式)
```json
{
    "profile" : {
        "name" : "user1",
        "gender" : "F",
        "age" : 18
    }
}
```


新增完會自動產生唯一的objectID，透過工具也可以對資料做簡單的修改動作。

# 3.mongo shell
- 可以再有提供shell指令視窗的工具上下指令去做更新進階的搜尋，
- 例如mongo 3T上方就有視窗可以使用．

------------


## 搜尋指令
#### 搜尋全部
db.getCollection('user').find({}) 
#### 搜尋by_id
db.getCollection('user').find({_id:ObjectId("5eaa780f3dfac43981e4412c")})

#### 限制顯示的欄位: 1可見,0不可見
db.getCollection('user').find({},{profile.name:1})

有一點像是
SELECT profile.name FROM user
_id預設顯示，可以關掉_id:0 ,

#### 搜尋內容值
db.getCollection('user').find({"profile.name":"user1"})
#### 搜尋範圍 數字型態  
[gt大於，gte大於等於，lt小於，lte小於等於]
db.getCollection('user').find({"profile.gender":"F","profile.age":{$gt:2,$lte:18}})
#### 搜尋範圍 字串型態 
按UTF-8進行字典排序 表搜尋字母A~Z間
db.getCollection('user').find({"profile.name":{$gt:"a",$lte:"z"}})

#### 排序
依命名排序，其中 1 为升序排列，而 -1 是用于降序排列
db.getCollection('user').find({}).sort({name:1})

#### 顯示筆數(limit)與開始(skip)
可以用他來做分頁讀取
db.getCollection('user').find({}).sort({name:1}).skip(0).limit(2)


------------


### 計算總數
db.getCollection('user').find().count()

------------


## 批次更新 
一般來說工具就可以做內容更新，但如果需要透過指令，
#### 更新欄位
將性別欄位“F”全改成“woman”
```bash
db.getCollection('user').find().forEach( function(d) {
     
if(d.profile.gender=="F"){
    d.profile.gender="woman"
    db.getCollection('user').update({"profile.name":d.profile.name},d);
    print(d.profile.name+" process change done" );  
};
});
```


#### 新增欄位合併內容

```bash
db.getCollection('user').find().forEach( function(d) {
d.description= d.profile.name+","+d.profile.gender;
db.getCollection('user').update({"profile.name":d.profile.name},d);
    print(d.profile.name+" process change done" );  
});
```

####  刪除欄位
```bash
db.getCollection('user').find().forEach( function(d) {
delete d.description
db.getCollection('user').update({"profile.name":d.profile.name},d);
    print(d.profile.name+" process change done" );  
});
```

------------




- 如果是要在mongo的docker環境上面下指令
需要把指令包成js，檔案丟進去docker裡面，
然後下指令
```bash
load("script/Change.js") //檔案位置
mongo 127.0.0.1:27017/test Change.js
```
執行結果與上述內容一樣


------------
本章對mongo的簡單操作到此結束，
之後再介紹如何用golang程式去對mongo CRUD操作．
