---
title: "[go]使用for-select-case 注意事項"
date: 2020-06-22T15:56:24+08:00
categories: [" 後端 go"]
tags: ["golang", "Channel"]
draft: false
---

 

#### 使用for-select-case 注意事項


這一天在查詢 select 用法看到這一篇文章:[golang select 的温柔陷阱](https://zhuanlan.zhihu.com/p/91044663 "golang select 的温柔陷阱")

- 才發現完成case執行時間也是會依照其他case而影響，
- 因此這篇最後有提醒:請盡量在返回chan的函数中減少耗時操作
- 另外case 在一起準備好的情況下，是隨機執行的，避免顺序敏感的情形
<!--more-->
改寫這個Go的炸彈爆炸範例 [Default Selection](https://tour.golang.org/concurrency/6 "Default Selection") 來驗證這個問題，故意在某個case sleep 長一點的時間，會發現BOOM的印出時間被延長了!!
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(100 * time.Millisecond)
	tick2 := time.Tick(200 * time.Millisecond)
	boom := time.After(500 * time.Millisecond)
	start := time.Now()
	for {
		select {
		case <-tick:
			fmt.Println("tick.100")
			fmt.Println("tick costs:ms", time.Since(start).Milliseconds())
		case <-tick2:
			fmt.Println("tick.200")
			fmt.Println("tick2 costs:ms", time.Since(start).Milliseconds())
		case <-boom:
			fmt.Println("BOOM!")
			fmt.Println("BOOM costs:ms", time.Since(start).Milliseconds())
			return
		default:
			fmt.Println("    .")
			time.Sleep(300 * time.Millisecond) //一開始這邊睡了300ms 導致tick/tick2都準備好隨機執行了
		}
	}
}


```