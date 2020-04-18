---
title: "[Go]http server-REST API"
date: 2020-04-18T10:30:25+08:00
draft: false
tags: ["golang"]

---
> 透過gin框架可以快速寫一個伺服器

效果：
http://localhost:8080/v1/word/jenny
出現 hello jenny

main.go
```go
func main() {
	rest.InitRouter()
}
```
rest/rest.go
```
package rest
import "github.com/gin-gonic/gin"

func InitRouter() {
	router := gin.Default()
	v1 := router.Group("/v1")
	{
		v1.GET("/word/:name", getword)
	}
	router.Run(":8080")
}
```
rest/func.go
```
func getword(c *gin.Context) {
	name := c.Param("name")
	c.String(http.StatusOK, "Hello %s", name)
}
```




