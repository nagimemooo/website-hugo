---
title: "[React] react JSX 基本語法說明"
date: 2020-05-30T18:32:15+08:00
lastmod: 2020-05-30T18:32:15+08:00
author: Nagi
categories: ["前端"]
tags: ["react", "jsx"]
draft: false
---
> 上一篇提到建立一個基本專案，這次來做一些介紹，及對index.js做一些改寫練習。


#### 參考網址:
[【React.js入門 - 06】 JSX](https://ithelp.ithome.com.tw/articles/10216468 "【React.js入門 - 06】 JSX") @IT邦幫忙鐵人分享

[React篇: JSX語法撰寫指引](https://eyesofkids.gitbooks.io/react-basic-zh-tw/content/day18_deeper_jsx/ "React篇: JSX語法撰寫指引")

------------


#### html 區域
public/index.html<br>
含有基本HTML範本id="root"的div區塊<br>

------------


#### React 中的 JSX 區域
src/index.js
基本範例1: <> html 部分
```javascript
ReactDOM.render(<App />, document.getElementById('root'));
或是
ReactDOM.render(<h1> Hello world!</h1>,
document.getElementById('root'));
```
- 由 React DOM 函式將元素渲染 ROOT 這個DOM 節點中
- 而將 html當參數傳遞是使用一種Javascript語法: JSX


------------
範例2: {JS}

```javascript
//js函式宣告或是變數宣告區
const styleRed = { color: 'red' };
const pic=()=>{
  return (
  <div><img src="https://picsum.photos/200/200?image=229" alt="" class="circle-profile"/></div>);
}
var arr = [
        <h1>REACT學習</h1>,
        <h2>如何使用JSX！</h2>,
];
ReactDOM.render(
<React.StrictMode>
   <h1 style = { styleRed } > Hello, world! </h1>
   <div>{ pic(} </div> {/*註解這樣寫*/}
   <div>{arr}</div>,{/*可以放入數組*/}
 </React.StrictMode>,
    document.getElementById('root')
);
```
- 可在 html 標籤中利用 {} 寫 javascript 表示式
- 其中style = {{ color: 'red' }} 這樣的表示也可以。

------------
範例3: 屬性類
```javascript
const getValue=(event)=>{
  console.log(event.target.value)
}
ReactDOM.render(
<React.StrictMode>
    <h1 className = "title" > Hello, world! </h1>
	 <button value onClick={getValue}>按下以取得數值 </button>
    <button value={true} onClick={getValue}>按下以取得數值 </button>
</React.StrictMode>,
    document.getElementById('root')
);
```
- 駱駝式命名
	- class 要用 className 然後可以在 style.css中更改樣式
	- onclick 也要改onClick{函數名稱} 駱駝式命名
	- 實測命名打錯 console 會出現報 Warning: Invalid DOM property `class`. Did you mean `className`?
- 輸入類的元件button/input/textarea互動事件觸發時，函式只會接收到一個event類別的參數，並不能傳遞其他參數
- 布林=true 的屬性值可以不寫

------------






