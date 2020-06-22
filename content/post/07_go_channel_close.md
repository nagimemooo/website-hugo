---
title: "[go]練習Channel的關閉與否判斷"
date: 2020-06-22T15:35:55+08:00
categories: [" 後端 go"]
tags: ["golang", "Channel"]
draft: false
---


> 練習Channel的關閉與否判斷

本章內文
- Buffered Channels
- close Channel
- 判斷關閉ok 與range差異
<!--more-->


#### go練習場
[Buffered Channels](https://tour.golang.org/concurrency/3 "Buffered Channels")
[Range and Close](https://tour.golang.org/concurrency/4 "Range and Close")

#### 參考文章
[Go中的Channel——range和select  Kenshinsyrup](https://www.jianshu.com/p/fe5dd2efed5d "Go中的Channel——range和select  Kenshinsyrup") range和select解說
[[Go] Goroutines 使用 Channel 交換資料](https://mileslin.github.io/2020/01/Golang/Goroutines-%E4%BD%BF%E7%94%A8-Channel-%E4%BA%A4%E6%8F%9B%E8%B3%87%E6%96%99/ "[Go] Goroutines 使用 Channel 交換資料")  channel 開關的範例

#### 範例：
- close()可以用來關閉c，關閉後的c值為0
- x, ok := <-c //將C接收賦值給x，與判斷c是否關閉
```go
package main
import (
	"fmt"
)
func main() {
	c := make(chan int, 10)
	c <- 1 //會跳到接收端
	c <- 2
	close(c)
	fmt.Println(<-c)
	fmt.Println(<-c)
	fmt.Println(<-c) //被關掉c就會是0

	x, ok := <-c //將C接收賦值給x，與判斷c是否關閉
	if ok {
		fmt.Println("Channel is open!")

	} else {
		fmt.Println("Channel is closed!")
	}
	fmt.Println("x is ", x)
}
```

改寫
- Go提供了range關鍵字，會自動等待channel的動作一直到channel被關閉
```go
func main() {
	c := make(chan int, 10)
	c <- 1 //會跳到接收端
	c <- 2
	c <- 3
	close(c)
	for s := range c { //不用fmt.Println(<-c)來接，不知道數量缺乏彈性
		fmt.Println("Packing received cake: ", s)
	}
}
```

另一個範例
-  傳送端做事，做完記得close，接收端用range接收並等待
```go
package main
import (
    "fmt"
    "strconv"
	//"time"
)
func worker(cs chan string, count int) {
    for i := 1; i <= count; i++ {
        cs <- "worker do" + strconv.Itoa(i)+" finish."
    }
    close(cs) //接收端用range等待作法要記得關閉傳送端
}
func receiveWorkerNotify(cs chan string) {
    for s := range cs {
        fmt.Println("Received notify: ", s)
    }
}
func main() {
    cs := make(chan string)
    go worker(cs, 5)
   receiveWorkerNotify(cs)  //接收端不能用go，要等待，不然main會結束
	//time.Sleep(4 * time.Second)
}
```


