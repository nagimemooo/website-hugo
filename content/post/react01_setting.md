---
title: "[React] 初始開發環境設定與名詞介紹"
date: 2020-05-16T15:42:53+08:00
lastmod: 2020-05-16T15:42:53+08:00
author: Nagi
categories: ["前端"]
tags: ["react", "vscode"]
draft: false
---

本章內容：
快速安裝 React 專案與建置開發環境

<!--more-->

#### 安裝node.js (npm)
- Node.js是能執行JavaScript 的執行環境，讓JS可以在伺服器(瀏覽器以外)運作
- npm（全稱 Node Package Manager，即「node包管理器」）是Node.js預設的、用JavaScript編寫的軟體套件管理系統

#### 使用 npm 安裝create-react-app
```bash
npm install -g create-react-app
/usr/local/bin/create-react-app -> /usr/local/lib/node_modules/create-react-app/index.js
+ create-react-app@3.4.1
~ create-react-app --version
3.4.1
```
-  -g 代表全局安裝
- create-react-app 是適合學習 React 的環境及單頁（single-page）應用程式，不需再安装或配置 Webpack 或 Babel 等工具， 它們是預先配置好並隐藏的
-  create-react-app --version 確認版本的指令- 

#### 使用create-react-app 建立專案
```bash
～create-react-app my-react-demo
Creating a new React app in /Users/xxx/front/my-react-demo.
```

#### 啟動專案
```bash
npm start
```


public/index.html
基本的HTML架構，內有```html
"<div id="root"></div>"
```這樣的內容


#### vscode 外掛
- ESlint 插件 ->程式碼規範檢查
- JS JSX Snippets  插件  ->程式碼快速鍵

------------


#### 延伸閱讀 [wait]
[有看沒懂的npx方式](https://www.itread01.com/content/1544755994.html "有看沒懂的npx方式")


