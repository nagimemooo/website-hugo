---
title: "[Ｇo] 安裝環境 MAC Note"
date: 2020-04-18T10:30:25+08:00
draft: false
tags: ["golang"]

---
> 之前在windows環境下安裝過go環境，這次在ＭＡＣ環境也裝一次並記錄
> 1.安裝官方ＧＯ：https://golang.org/
> 2.配置Ｇo所需要環境變數
> 3.找個喜歡的ＩＤＥ設定開始寫Ｇo 「推薦Visual Studio Code」
> 因為網上已經不少文章關於這些說明，所以簡單記錄幾個keyword與操作

**1.ＧＯ網站下載直接點擊安裝**
會幫你安裝到/usr/local/go底下

**2.配置ＭＡＣ環境變數**

筆者選擇的是用在使用者目錄下配置環境變數
vi ~/.bash_profile
```
輸入a編輯
export GOROOT="/usr/local/go"
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin 
export PATH=$PATH:$GOROOT/bin
export GO111MODULE="on"

打完後esc輸入：wq存擋
```
執行 bash profile
source ~/.bash_profile

GOROOT表示GO安裝的目錄
GOPATH是自訂想要放置程式的地方，筆者這邊是放在＄HOME/新建go目錄
底下又分三個目錄（pkg/bin/src），bin 可執行文件，src 則是放置程式source code。
GO111MODULE=on 
啟用module功能讓第三方套件在GOPATH/pkg/mod下去尋找

開啟終端機下指令確認安裝
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

*如果要重新安裝*
也是要把/usr/local/go刪除，環境變數拿掉，再安裝一次動作。

延伸閱讀-寫的超棒的文章
[Golang — GOROOT、GOPATH、Go-Modules-三者的關係介紹](https://medium.com/%E4%BC%81%E9%B5%9D%E4%B9%9F%E6%87%82%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88/golang-goroot-gopath-go-modules-%E4%B8%89%E8%80%85%E7%9A%84%E9%97%9C%E4%BF%82%E4%BB%8B%E7%B4%B9-d17481d7a655)

3.IDE推薦
下載Visual Studio Code
安裝Go外掛+安裝依賴包支援
[Mac上Go環境和VS Code的正確安裝與配置方法](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/391186/)


I have a requirement for recording  "case *ua.DataChangeNotification" time in millisecond , Is it possible to get  publishtime ?



