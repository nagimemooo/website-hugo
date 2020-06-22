---
title: "[go]練習Channel select的用法"
date: 2020-06-22T15:52:56+08:00
categories: [" 後端 go"]
tags: ["golang", "Channel"]
draft: false
---

 > 練習Channel select的用法
 
<!--more-->

#### 參考資料:
- [Go中的Channel——range和select  Kenshinsyrup](https://www.jianshu.com/p/fe5dd2efed5d "Go中的Channel——range和select  Kenshinsyrup") range和select解說
- https://zhuanlan.zhihu.com/p/91044663 golang select 的温柔陷阱
- [Default Selection](https://tour.golang.org/concurrency/6 "Default Selection") @go playground一般select用法(time.Tick&time.After)
- [Go 語言使用 Select 四大用法](https://blog.wu-boy.com/2019/11/four-tips-with-select-in-golang/ "Go 語言使用 Select 四大用法")

#### select特性
- select用於等待多個"channel"的结合，只能接 channel 否則會出錯。
- select 的特性之一，case 是隨機選取。
- 如果有多個 channel 需要讀取，而讀取是不間斷的，就必須使用 for + select 機制實現，但要注意for的跳出條件(return出迴圈)

將上次的範例改寫:
改成兩個gorotine同時執行，並接收不同go回傳的結果

```go
package main

import (
    "fmt"
    "strconv"
	//"time"
)

func worker(cs chan string,desc string, count int) {
    for i := 1; i <= count; i++ {
        cs <- "worker(" +desc+") work num#"+ strconv.Itoa(i)+" do finish."
    }
    close(cs) //接收端用range等待作法要記得關閉傳送端
}

func receiveWorkerNotify(work1_cs chan string,work2_cs chan string) {
      work1_closed, work2_closed := false, false

    for {
        //if both channels are closed then we can stop
        if work1_closed && work2_closed {
            return
        }
        fmt.Println("Waiting for a new task ...")
        select {
        case taskDesc, ok := <-work1_cs:
            if !ok {
                work1_closed = true
                fmt.Println(" ... work1_cs channel closed!")
            } else {
                fmt.Println("Received from work1_cs channel. taskDesc:", taskDesc)
            }
        case taskDesc, ok := <-work2_cs:
            if !ok {
                work2_closed = true
                fmt.Println(" ... work2_cs channel closed!")
            } else {
                fmt.Println("Received from work2_cs channel.  Now taskDesc:", taskDesc)
            }
        }
    }
}

func main() {
    work1_cs := make(chan string)
	work2_cs := make(chan string)
    go worker(work1_cs,"work1_cs", 5)
	go worker(work2_cs,"work2_cs", 5)
    receiveWorkerNotify(work1_cs,work2_cs)  //接收端不能用go，要等待，不然main會結束
	//time.Sleep(4 * time.Second)
}
```
#### default區塊
- select中也可以有default區塊，其一直是準備好的，且没有任何case區塊準備好，執行default區塊
- select + default 方式可用來來確保 channel 是否已滿

```go

func main() {
	ch := make(chan int, 1)//改看看
	ch <- 1
		select {
		case ch <- 2:
			fmt.Println("channel value is", <-ch)
			fmt.Println("channel value is", <-ch)
		default:
			fmt.Println("channel blocking")
		}
	
}
```
#### timeout機制
- 為避免等不到接收值，可以在select中加上timeout機制或time.After
- 如果沒有timeout則會all goroutines are asleep - deadlock!

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int, 2)
	//ch <- 1 //不傳送值

	select {
	case x := <-ch:
		fmt.Println("channel value is", x)
	case <-time.After(time.Second * 1):
		fmt.Println("timeout")

	}
	fmt.Println("end")
	}

```

