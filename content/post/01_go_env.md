+++
author = "nagi"
title = "[Go 01] 配置GO開發環境只要三步驟之筆記"
date = "2019-03-11"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "golang",
]
categories = [
    "後端 go"
]
series = ["golang Guide"]
aliases = ["migrate-from-jekyl"]
+++

## 配置GO開發環境三步驟
1. 安裝官方ＧＯ：https://golang.org/ 
1. 配置Ｇo所需要環境變數 
1. 找個喜歡的ＩＤＥ設定開始寫Ｇo 「推薦Visual Studio Code加上插件」

<!--more-->
> 網路上已經有很多圖文版本教學，都很詳細，只要做完以後三步驟就可以開心寫ＧＯ囉！因此本文只是純記錄操作內容與分享參考文章～


#### 直接看參考文章安裝: 
windows 安裝Go
- [[Go] Go 語言於 Windows 上之安裝與環境設定](https://oranwind.org/go-go-yu-yan-yu-windows-shang-zhi-an-zhuang-yu-huan-jing-she-ding/ "[Go] Go 語言於 Windows 上之安裝與環境設定") 

Ｍac 安裝Go
- [Mac上Go環境和VS Code的正確安裝與配置方法](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/391186/) 

IDE:VScode
- [[Go] 使用 Visual Studio Code 上建置 Go 開發環境](https://oranwind.org/go-ide-visual-studio-code/ "[Go] 使用 Visual Studio Code 上建置 Go 開發環境") 


------------
以下記錄本人的安裝筆記

### MAC安裝筆記

#### 1.ＧＯ網站下載直接點擊安裝
預設會幫你安裝到/usr/local/go底下，這就是ＧＯＲＯＯＴ位置
#### 2.配置ＭＡＣ環境變數

筆者選擇的是用在使用者目錄下配置環境變數
- vi ~/.bash_profile
```
輸入a編輯
export GOROOT="/usr/local/go"
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin 
export PATH=$PATH:$GOROOT/bin
export GO111MODULE="on"
```
打完後esc輸入：wq存擋

- 執行 bash profile
	source ~/.bash_profile

- 說明：GOROOT表示GO安裝的目錄
GOPATH是自訂想要放置程式的地方，筆者這邊是放在＄HOME/新建go目錄
底下又分三個目錄（pkg/bin/src），bin 可執行文件，src 則是放置程式source code。
GO111MODULE=on 
啟用module功能讓第三方套件在GOPATH/pkg/mod下去尋找

- 開啟終端機下指令確認安裝
```
➜  ~ echo $HOME
/Users/xxx
➜  ~ go version
go version go1.14 darwin/amd64
➜  ~ which go  
/usr/local/go/bin/go
➜  ~ go env
會列出上述設定可以確認
```

#### 註:如果要重新安裝
也是要把/usr/local/go刪除，環境變數拿掉，再安裝一次動作。


#### 3.IDE 安裝
- 下載 官網Visual Studio Code
- 打開ＶＳcode於Extensions安裝Go外掛
- 打開終端機執行下列指令來安裝依賴包以下工具:
```bash
go get -u -v github.com/ramya-rao-a/go-outline
go get -u -v github.com/acroca/go-symbols
go get -u -v github.com/mdempsky/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v golang.org/x/tools/cmd/godoc
go get -u -v github.com/zmb3/gogetdoc
go get -u -v golang.org/x/lint/golint
go get -u -v github.com/fatih/gomodifytags
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v sourcegraph.com/sqs/goreturns
go get -u -v golang.org/x/tools/cmd/goimports
go get -u -v github.com/cweill/gotests/...
go get -u -v golang.org/x/tools/cmd/guru
go get -u -v github.com/josharian/impl
go get -u -v github.com/haya14busa/goplay/cmd/goplay
go get -u -v github.com/uudashr/gopkgs/cmd/gopkgs
go get -u -v github.com/davidrjenni/reftools/cmd/fillstruct
```
> 配置好後在編輯ＧＯ語言就會發現有很多貼心的提示:grinning:，同時也會出現在ＩＤＥ內的problems清單裡，可以進一步修改語法，真的是超級方便的．以下是更多相關工具的用途說明

|工具   |  說明 |
| ------------ | ------------ |
| dlv.exe	go  | 语言调试工具  |
| gocode.exe	go  |  语言检查，自动补全 |
| godef.exe 	go  |  语言定义和引用的跳转 |
| golint.exe 	go  | 语言规范检查  |
| go-outline.exe  |  用于在Go源文件中提取JSON形式声明的简单工具 |
|  gopkgs.exe |   	快速列出可用包的工具 |
| gorename.exe  |  在Go源代码中执行标识符的精确类型安全重命名 |
|  goreturns.exe | 类似fmt和import的工具，使用零值填充Go返回语句以匹配func返回类  |
| go-symbols.exe  |  	从go源码树中提取JSON形式的包符号的工具  |
| gotour.exe 	  | go语言指南网页版  |
| guru.exe  |  	go语言源代码有关工具，如代码高亮等  |