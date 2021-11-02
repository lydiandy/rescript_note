## 异常/错误处理 exception

### 内置异常

```ocaml
Not_found		//找不到异常
Invalid_argument 	//无效参数异常
...
```

### 自定义异常

除了使用各种内置的异常外,也可以自定义异常

```ocaml
exception MyException(string)
```

### 抛出异常

```ocaml
raise(MyException("this is exception"))
```

### 捕获异常

```ocaml
exception MyException(string) //自定义异常

let myfn = (i: int) => {
  Js.log("from myfn")

  switch i {
  | 1 => Js.log("ok,i>0") //未抛出异常
  | 0 => raise(Not_found) //抛出内置异常Not_found
  | -1 => raise(MyException("this is my exception")) //抛出自定义异常
  | _ => Js.log("other") //穷尽其他情况,下划线忽略其他情况
  }
}

try {
  Js.log("from try")
  myfn(-1) //调用函数后,如果触发异常,跳转到catch代码块
} catch {
	//捕获异常代码块,类似于switch语句,模式匹配
	| Not_found => Js.log("Not_found")
	| MyException(content) => Js.log(content) //如果某个异常没有捕获到,	则程序终止,报错
}

```

### try/catch作为表达式

```ocaml
let result = 
  try { //作为表达式返回
    getItem([1, 2, 3])
  } catch {
  | Not_found => 0 // 返回默认值，如果调用的getItem抛出异常
  }
```

### 捕获js异常

```ocaml
try {
  someJSFunctionThatThrows()
} catch {
| Js.Exn.Error(obj) =>
  switch Js.Exn.message(obj) {
  | Some(m) => Js.log("Caught a JS exception! Message: " ++ m)
  | None => ()
  }
}
```

### 在js中捕获rescript异常

在rescript中定义异常，抛出异常：

```ocaml
exception BadArgument({myMessage: string})

let myTest = () => {
  raise(BadArgument({myMessage: "Oops!"}))
}
```

在js代码中捕获异常：

```js
try {
  myTest()
} catch (e) {
  console.log(e.myMessage) // "Oops!"
  console.log(e.Error.stack) // the stack trace
}
```

