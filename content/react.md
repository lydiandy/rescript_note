## ## 集成react

https://github.com/rescript-lang/rescript-react

文档: https://rescript-lang.org/docs/react/latest/introduction

### 功能总览

- 不需要babel插件（JSX就是rescript语言中的一部分）

- 绑定到react的最新版本,主要的API都包含,特别是react hooks API

- 没有类组件API(只有函数组件,react hooks)

- 类型安全,组件属性,状态值都是

- GenType 支持导入/导出React Component 的Flow和Typescript类型声明

### 安装

```shell
npm install @rescript/react --save
```

添加到bsconfig.json

```json
{
  "reason": { "react-jsx": 3 },
  "bs-dependencies": ["@rescript/react"]
}
```

简单demo:

```react
@@config(no_export)

let {string} = module(React)
let {render, querySelector} = module(ReactDOM)
module App = {
    @react.component
    let make = () => {
  <div> {string("Hello World")} </div>
}
}

switch querySelector("#root") {
| None => ()
| Some(e) => render(<App />, e)
}
```

### 组件定义

在rescript中，一个组件的定义(组件接口)是：一个模块(或子模块)，包含有make函数和makeProps函数,并且make函数要返回JSX。为了简化，特别提供了@react.component注解来自动生成makeProps，最终的组件的结构就变成：

```ocaml
module Button = { //模块必须是大写开头，刚好也符合react组件命名规范
	@react.component
	let make = (~prop1,~prop2) => { //make可以看做是组件的构造函数
		
		<div>...</div>	//make函数返回JSX
	}
}
```

### JSX







### 集成recoil

https://github.com/bloodyowl/rescript-recoil

