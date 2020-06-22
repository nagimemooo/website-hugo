---
title: "[React] 練習class component改變State的用法"
date: 2020-05-30T18:36:32+08:00
lastmod: 2020-05-30T18:36:32+08:00
author: Nagi
categories: ["前端"]
tags: ["react", "class component","setState"]
draft: false
---

 > 練習React 改變State的用法
 
<!--more-->

#### 網路參考範例:

[React State(状态)](https://www.runoob.com/react/react-state.html "React State(状态)") @runoob基礎與線上範例
**[State 和生命週期](https://zh-hant.reactjs.org/docs/state-and-lifecycle.html "State 和生命週期") @React中文React解說
[【React.js入門 - 11】 開始進入class component](https://ithelp.ithome.com.tw/articles/10219057 "【React.js入門 - 11】 開始進入class component") @IT邦幫忙的系列文
[React 與 bind this ](https://medium.com/reactmaker/react-%E8%88%87-bind-this-%E7%9A%84%E4%B8%80%E4%BA%9B%E5%BF%83%E5%BE%97-323c8d3d395d "React 與 bind this ") @medium



- 使用 ES6 class來 來定義 
- 繼承React.Component且在用render(){}包一層
- props 要改用 this.props
- 如果想要更改props ，要改用state


````javascript
class 物件名稱  extends React.Component{
    constructor(props) {
    super(props);
    this.state={ 
        變數名稱A: 初始化值, 
        變數名稱B: 初始化值, ...
    };
	//this.changeXxx=this.changeXxx.bind(this);不用箭頭函式要加
  }
	//讀取
  ....this.state.變數名稱
	
	//改變，與讀取用法非常不同，一定要用以下格式才正確
	//changeXxx(){
	changeXxx = () => { //建議用箭頭函式改法
    this.setState({ 變數名稱A: 變化值 }) 
  }
  
    render(){
        return(<div>...</div>
		);
    }
}
````
範例練習:透過一個新的按鈕去改變時間 [Refresh Time](https://codesandbox.io/s/refreshtime-ju4pv?file=/src/index.js "Refresh Time")


````javascript
import React from "react";
import ReactDOM from "react-dom";

import App from "./App";

const rootElement = document.getElementById("root");

class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
    // this.changeTime=this.changeTime.bind(this);
  }

  // changeTime(){
  //   this.setState({date: new Date()})

  // }
  //根據React 與 bind this
  //https://medium.com/reactmaker/react-%E8%88%87-bind-this-%E7%9A%84%E4%B8%80%E4%BA%9B%E5%BF%83%E5%BE%97-323c8d3d395d

  //以上可以簡化 改箭頭含式寫法
  changeTime = () => {
    this.setState({ date: new Date() });
  };

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
        <button onClick={this.changeTime}>刷新 </button>
      </div>
    );
  }
}

ReactDOM.render(
  <React.StrictMode>
    <Clock />,
  </React.StrictMode>,
  rootElement
);

````



