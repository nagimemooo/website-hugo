---
title: "[ Git ]常用指令與名詞筆記"
date: 2020-04-18T10:30:25+08:00
draft: false
cover: /img/git.png
categories:
  - Tech筆記
tags:
  - git

---
> 本文文字版紀錄 git 常用使用筆記

<!--more-->

其實很少記得 git 指令，都是遇到些問題，長期獨立開發，所以很少用到merge．

* HEAD 表示「目前所在分支」
* 遠端的節點預設會使用 origin 這個名字

#### git初始化 將遠端與本地資料夾連結
通常在github新創repo都會有指令導引，照著新創建的指令去下即可連結

…or create a new repository on the command line
echo "# go_demo" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/nagimemooo/go_demo.git
git push -u origin master
                
…or push an existing repository from the command line
git remote add origin https://github.com/nagimemooo/go_demo.git
git push -u origin master

* 解說
git init : 初始化建一個.git文件夾是附屬於該倉庫的工作樹
git remote -v : 查看遠端位置
git remote add <remote> <url> : 新增遠端git位置
git remote remove <remote> : 移除遠端git位置
git push -u 遠端 本地


#### git 下載已存在遠端項目
新建資料夾後下指令
到遠端項目上複製git clone指令

#### git 上傳與分支操作
完成連結後，我還是習慣使用vscode去做git上傳操作
好處是可以快速切換分支
每次變動後可以在分割視窗review再上傳，
如果不更改也可以恢復更改

#### git status
查看狀態

#### git config
git config --list
git config --global user.email
git config --global user.email nagimemooo@gmail.com 
vi .git/config ·編輯內容（按下q離開）

--global 參數，代表在此系統，Git 做任何事都會採用此資訊。 
若想指定特定專案不同的設定就拿掉 global 參數。


#### git 刪除分支
git branch 查詢
git branch -d <branch> 刪除

#### 做錯 commit 還沒 push 時的恢復方法
$ git log --oneline
$ git reset 66cb4b3^
^表示上一步，＾＾表示上兩步，或直接打分支命名


#### git rebase
當有很多分支走不同路後，
希望某一個分支重新 (re-)定義某個 branch 的參考基準 (base)，
就可以使用．


