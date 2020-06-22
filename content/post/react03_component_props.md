---
title: "[React] component語法- props出場 (function vs class)"
date: 2020-05-30T18:34:33+08:00
lastmod: 2020-05-30T18:34:33+08:00
author: Nagi
categories: ["前端"]
tags: ["react", "component"]
draft: false
---


網路參考範例:
[React 组件](https://www.runoob.com/react/react-components.html "React 组件")
[Components 與 Props](https://zh-hant.reactjs.org/docs/components-and-props.html "Components 與 Props")


### React component (組件)語法

------------


#### function component
- 可以使用(function component)函式定義组件JSX
- ReactDOM.render 中{函式名稱}變成了<函式名稱/>
- 命名首字必須大寫
- 如果需要向component傳参数，可以使用 this.props 對象，
- props 通常是不可變的(唯獨)，不能修改自己的
- 如果想要更改props 要改用State (下一篇 [React] function component & useState )

````javascript
//function component
function HelloWorld() {
	return <h1>Hello World!</h1>;
}
function HelloName(props) {
	return <h1>Hello {props.name}!</h1>;
}
ReactDOM.render(
 <React.StrictMode>
	 <HelloWorld/>
	 <HelloName name="May"/>
	</React.StrictMode>,
	document.getElementById('example')
);
````

------------


#### 使用class來做 component
- 也可以使用 ES6 class來 來定義 
- 繼承React.Component且在用render(){}包一層
- props 要改用 this.props
- 如果想要更改props 要改用state (下一篇 [React] class component & setState )

````javascript
//class來 component
class HelloName extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

ReactDOM.render(
 <React.StrictMode>
	<HelloName name="May" />;,
	</React.StrictMode>,
	document.getElementById('example')
);
````

組件裡面可以再包組件，透過這樣可以重新利用
範例練習: [USER info](https://codesandbox.io/s/usercard-vh0e4 "USER info")




