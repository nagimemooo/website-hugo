---
title: "[Go 04] 使用 Gin 框架快速建立 http 服務"
date: 2020-05-09T00:00:05+08:00
lastmod: 2020-05-09T00:00:05+08:00
author: Nagi
cover: /img/go.png
categories: [" 後端 go"]
tags: ["golang", "gin"]
# showcase: true
draft: false
---

本章介紹：
* 如何用 Gin 框架建立 HTTP [ GET/POST/PUT/DELETE 等方法]

<!--more-->


#### 效果
用 Postman 工具打看看就可以得到下面結果

|  SEND [method url ]|  RESPONSE [status body ]  |
| :------------ | :------------ |
|  GET  http://localhost:8080/api/v1/user  | 200 , OK  |
|  GET http://localhost:8080/api/v1/user/May| 200 ,"Hello,May"  |
|  POST http://localhost:8080/api/v1/user <br>{"name": "user1","age": 33}| 200 , {"name": "user1","age": 33} |


#### 程式碼
```go
// 1.啟動服務
func StartHttpServer(errChan chan error) {
	gin.SetMode(gin.ReleaseMode)
	engine := gin.New()
	initRoutes(engine)
	go func() { errChan <- engine.Run(:8080) }()
}

// 2.設定路由組
func initRoutes(e *gin.Engine) {
	root := e.Group("api/v1")
	userGroup := root.Group("user")
	{
		userGroup.GET("", apis.User)
		userGroup.GET(":name", apis.UserName)
		userGroup.POST("", apis.PostName)
	}
}

// 3. 設定回覆
func User(c *gin.Context) {
	c.JSON(http.StatusOK, "OK") //回覆status 200 & body "OK"
}
//接受path參數
	name := c.Param("name")
	c.JSON(http.StatusOK, fmt.Sprintf("%s,%s", "Hello", name))
}

//接收 json 內容
func PostName(c *gin.Context) {
	sc := &structs.User{}
	if err := c.ShouldBindJSON(sc); err != nil {
		return
	}
	c.JSON(http.StatusOK, sc)
}
type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

```




#### Gin延伸
- 寫中間件 [控制每個Url timeout/log 等等行為]
- 套swagger [下一篇]
- gin.Context 還有 c.Header, c.Query, c.GetRawData() 等等使用方法，取參數非常方便

------
第一次去套用Gin真的覺得很神奇，本章純快速記錄效果，
如有不懂可以再去搜尋關鍵字，網路上也有很多介紹文章跟使用方法喔



