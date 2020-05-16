---
title: "[ Go 05 ] 寫測試並產生網頁版報告看覆蓋率"
date: 2020-05-09T00:02:42+08:00
lastmod: 2020-05-09T00:02:42+08:00
author: Nagi
cover: /img/go.png
categories: ["Go"]
tags: ["golang", "test"]
# showcase: true
draft: false
---

本章介紹：
-   為上一篇 Gin 框架的 User & PostName func.寫個簡單測試
-  跑測試並瞭解 coverage 覆蓋率，產生測試報告

<!--more-->


說明：新建一個檔案user_test.go，並未func取名TestＸＸＸ(t *testing.T)
如果用VS code，可以在上方看到 run test | debug test 按鈕可以按，十分方便
利用 t.Errorf 觸發錯誤
```go
//測試打GET /api/v1/user 去跑User() 會拿到“ＯＫ”
func TestUser(t *testing.T) {
	engine := gin.New()
	initRoutes_mock(engine)
	uri := "/api/v1/user"

	body := Get(uri, engine)
	fmt.Printf("response:%v\n", string(body))
	if !reflect.DeepEqual(string(body), "\"OK\"") {
		t.Errorf("Get user name need to be ok!")
	}
}
//同理測試 Post 去跑PostName() 會拿到回復的名字等於打的內容（body）
func TestPostName(t *testing.T) {
	engine := gin.New()
	initRoutes_mock(engine)
	uri := "/api/v1/user"
	user := structs.User{Name: "user", Age: 18}
	body := PostUser(uri, user, engine)
	fmt.Printf("response:%v\n", string(body))

	response := &structs.User{}
	if err := json.Unmarshal(body, response); err != nil {
		t.Errorf("Unmarshal，err:%v\n", err)
	}
	if response.Name != "user" {
		t.Errorf("response different，user:%v\n", response.Name)
	}
}

func Get(uri string, router *gin.Engine) []byte {
	req := httptest.NewRequest("GET", uri, nil)
	w := httptest.NewRecorder()

	router.ServeHTTP(w, req)
	result := w.Result()
	defer result.Body.Close()
	body, _ := ioutil.ReadAll(result.Body)
	return body
}

func PostUser(uri string, param structs.User, router *gin.Engine) []byte {
	jsonByte, _ := json.Marshal(param)
	req := httptest.NewRequest("POST", uri, bytes.NewReader(jsonByte))
	w := httptest.NewRecorder()
	router.ServeHTTP(w, req)
	result := w.Result()
	defer result.Body.Close()
	body, _ := ioutil.ReadAll(result.Body)
	return body
}

//把路由設定寫回來 因為一直遇到cycle問題
func initRoutes_mock(e *gin.Engine) {
	root := e.Group("api/v1")
	userGroup := root.Group("user")
	{
		userGroup.GET("", User)
		userGroup.GET(":name", UserName)
		userGroup.POST("", PostName)
	}
}


```
#### 測試單一個function
 cover有帶的話會算出覆蓋率，並要在該目錄下去執行，這邊跑出來結果大約有57.1% 的覆蓋
```bash
$go test -v -cover=true user_test.go user.go
PASS
coverage: 57.1% of statements
ok      command-line-arguments  0.297s  coverage: 57.1% of statements
```
#### 測試整個專案
```bash
如果是在main的目錄要往子目錄找
＄go test -v ./…
```
#### 產生測試報告
```bash
go test -coverprofile=coverage.out ./...
用gool tool
go tool cover -func=coverage.out
go tool cover -html=coverage.out
```
這個真的很酷，用網頁產生報告，而且非常視覺化，
可以看出剛剛沒有寫到的UserName()測試為紅色
![](/img/posts/test_coverage.png)


-----

當然寫測試還有很多判斷的條件等等，是否等於，是否不等於，各種輸出可能，
雖然也會花上一些時間，但寫測試是為了避免之後維護及更改不小心動到邏輯等，
有時間的話還是要把這段補上喔！（一開始覺得有些範例太難寫，到後來越來越沈重...QQ）


參考文章：  
[基于golang gin框架的单元测试](https://studygolang.com/articles/11836 "基于golang gin框架的单元测试")
