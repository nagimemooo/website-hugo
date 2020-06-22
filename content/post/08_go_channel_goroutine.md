---
title: "[go]練習Goroutines 與Channel的傳送與接收"
date: 2020-06-22T15:41:33+08:00
categories: [" 後端 go"]
tags: ["golang", "Channel"]
draft: false
---


> 練習Goroutines 與Channel的傳送與接收

本文內文：
- 練習Goroutines 與Channel的傳送與接收
- 調整Buffered Channel來查看差異
<!--more-->

#### go練習場
[Channels](https://tour.golang.org/concurrency/2 "Channels") @Go Playground

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	worklist := []int{7, 2, 8, -9, 4, 0}
	c := make(chan int)
	go worker1(worklist[:len(worklist)/2], c)
	go worker2(worklist[len(worklist)/2:], c)
	time.Sleep(4 * time.Second)
	x, y := <-c, <-c // receive from c

	fmt.Println("received work result", x, y, x+y)
	time.Sleep(2 * time.Second)//關門前再等等
}//end of main
func worker1(s []int, c chan int) {
	fmt.Println("worker1 working")
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum                      // send sum to c
	fmt.Println("worker1 Finish") //
}

func worker2(s []int, c chan int) {
	fmt.Println("worker2 working")
	sum := 0
	//fmt.Println("worker2 Sleep 3 Second")
	//time.Sleep(3 * time.Second)

	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
	fmt.Println("worker2 Finish")
}


//fmt.Println("workerX Finish") c沒有buffer，這邊會等待被讀取才會做，如果main太快結束這邊也不會做
//fmt.Println("workerX Finish") c有buffer(1)，第一個做完會印，第二個人要等被讀取才會做
//fmt.Println("workerX Finish") c有buffer(2)，兩個人都會馬上印，不需等待被讀取

```

說明:
- 主程式工廠開始(main)，員工只能在工廠開門時做完事(end of main)
- 老闆交代一堆任務(worklist)，定義一部不會被占線的電話(c, no buffer)
-將前半部交給員工1，給一部電話，員工1就去做了。後半部給員工2，也是同一部電話。
- 交代兩人做完結果要用電話通知傳送(c <- sum)。
- 老闆則是等著兩個人電話通知結果才會結束工廠關門(end of main)。
- 員工如果做電話通知之後還有事情要做(ex: Println("Finish"))，必須要在工廠結束前趕快做，不然就做不到。




