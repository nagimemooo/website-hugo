---
title: "[Go 03] 信號處理和退出程式"
date: 2020-05-01T22:36:13+08:00
lastmod: 2020-05-01T22:36:13+08:00
author: Nagi 傷腦筋
cover: /img/go.png
categories: [" 後端 go"]
tags: ["golang", "signal"]

---

本文教學：
- go實作接收命令而中止程式．
- 會用到channel管道來進行阻塞，並接收os/signal訊號

<!--more-->

> 一般在執行go run main.go後就會馬上回到命令列
> 這邊實作當接收到ctrl+c或是終止程式才會停止程式



上程式碼：

```
func main() {
	fmt.Println("start")
	errs := make(chan error, 1)
	listenForＳignal(errs)
	c := <-errs  //阻塞程式
	fmt.Println("terminating:", c)
}

func listenForＳignal(errChan chan error) {
	go func() {
		c := make(chan os.Signal)
		signal.Notify(c, syscall.SIGINT, syscall.SIGTERM)//要終止的訊號

		errChan <- fmt.Errorf("%s", <-c)
	}()
}

```

說明：
 * 使用"os/signal"包
用來接收訊號使用，notify方法用来監聽收到的信號（stop方法則取消）
    * SIGINT	表示用户按下INTR字符(Ctrl+C)觸發
    * SIGTERM	结束程序 kill pid的作用是向進程為pid的程序发送SIGTERM
    * 其他像是SIGKILL   kill -9 pid則是發送立即終止 等等就先不使用



#### 測試接收SIGINT

然後執行go run main.go後，會看到服務就一直執行著，再按下ctrl+c

```
> go run main.go
start
^Cterminating: interrupt
```

#### 測試接收SIGTERM
先將main.go編譯成執行檔 -o代表放在目前目錄下 取名為demo
"./"執行demo這檔案
```
go build -o ./demo  main.go
 ./demo 
start
```

接下來開另一視窗 找出進程跟demo有關的pid 然後執行kill pid，確認已停止了
```
➜  ~ ps -A  | grep demo     
14693 ttys000    0:00.00 ./demo
➜  ~ kill 14693
➜  ~ ps -A  | grep demo
```

回到程式執行視窗就會看到以下被中止的訊息了

```
 ./demo
start
terminating: terminated
```

------------


後記：不太知道到底要怎麼要在vscode debug模式
去模擬ctrl+c時會跑到的地方來看程式，google未有結果，無解
