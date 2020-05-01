---
title: "[git]常用指令與名詞筆記"
date: 2020-04-18T10:30:25+08:00
draft: false
cover: /img/git.png
categories:
  - Tech筆記
tags:
  - git

---
> 本文文字版紀錄git常用使用筆記

<!--more-->



#### git 資料夾連結
切換到本地項目地址git init初始化項目。
該步驟會創建一個.git文件夾是附屬於該倉庫的工作樹
通常在github新創repo都會有指令導引，照著新創建的指令去下即可連結

#### git 下載
到遠端項目上複製git clone指令，新建資料夾後下指令

#### git 上傳與分支操作
完成連結後，我還是習慣使用vscode去做git上傳操作
好處是可以快速切換分支，每次變動後可以再分割視窗review再上傳，
如果不更改也可以恢復更改

#### git 刪除分支
> git branch 查詢
> git branch -d <branch> 刪除

#### 做錯commit的回覆方法

>$ git log --oneline
>$ git reset 66cb4b3^
^表示上一步，＾＾表式上兩步，或直接打分支命名

* HEAD表示「目前所在分支」，遠端的節點預設會使用 origin 這個名字

#### git rebase
當有很多分支走不同路後，希望某一個分支重新 (re-)定義某個 branch 的參考基準 (base)，就可以使用，如有需要請小心謹慎使用．

後記：長期都是獨立開發，所以rebase/merge使用較少，就記錄到這邊．
