---
title: "Github_hugo"
date: 2020-05-01T18:42:55+08:00
draft: false
---

> 本章介紹如何在github套用hugo主題做好一個自己的部落格網站．
> 文章來源整理：
> [ＨＵＧＯ官網](https://gohugo.io/getting-started/quick-start/)
> [在-github-部署-hugo-靜態網站](https://medium.com/@chswei/%E5%9C%A8-github-%E9%83%A8%E7%BD%B2-hugo-%E9%9D%9C%E6%85%8B%E7%B6%B2%E7%AB%99-9c40682dfe40)
> 

<!--more-->
#### Step 1: 安裝Hugo
開啟終端機，依序執行下列指令：
* brew install hugo 
//macOs 須先安裝Homebrew
* hugo version 
//確認安裝成功版本
Hugo Static Site Generator v0.69.0/extended darwin/amd64 BuildDate: unknow
* hugo new site **website-hugo**
* cd **website-hugo**
// 新增網站，粗體可以自命名，安裝完會新增該資料夾


#### Step 2: 新增主題
https://themes.gohugo.io/ 
hugo網站有很多可以選擇與查看效果
進入該主題的github/README 可以看安裝步驟執行
基本執行：git submodule add https://github.com/alex-shpak/hugo-book themes/book
//這樣就會在**website-hugo**/themes/新增主題

#### Step 3: 編輯 config.toml
這檔案與整體網站設定有關
> baseURL = "https://xxx.github.io/"    
> languageCode = "zh-tw"
> title = "xxx Blog"
* #domain設定 xxx改成你的GitHub帳號名稱
* 根據主題不同 這檔案也可能會有更多不同設定
   ex: 設定主題 theme = "ananke"

#### Step 4: 新增文章
* hugo new posts/my-first-post.md
新增預設檔案在以下位置 採用markdown編寫
content/<CATEGORY>/<FILE>.<FORMAT>
> title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true //是草稿是否 改成false可被發布

可以根據主題新增：
tags: ["hugo", "web"]
summary: "The summary image should be a custom one"
summaryImage: "summary_2.jpg"
resources:
- src: summary_2.jpg

#### Step 5: 啟動本地server

*  hugo server -D
Web Server is available at http://localhost:1313/
Press Ctrl+C to stop
記得結束務必按 不然下次啟動
#### Step 6: Build static pages

* hugo -D

將會生成./public/ 資料夾，
每次編輯完要記得更新，之後發布的時候也要上傳


-----


### githug網站上傳

新增兩個repo
xxx.github.io (xxx改成自己的帳號名稱)
website-hugo 上述的site名稱

上傳public資料夾
> cd public
git init
git remote add origin https://github.com/xxx/xxx.github.io.git
git add .
git commit -m "Initial commit"
git push -u origin master

上傳整個website-hugo資料夾
> cd ../
git init
git remote add origin https://github.com/xxx/website-hugo.git
git add .
git commit -m "Initial commit"
git push -u origin master


開啟https://xxx.github.io/ 看到結果


