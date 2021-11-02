## 变量

### let绑定

let把值(value)跟名字(name)绑定在一起，绑定后，代码就可以通过名字来访问值。

在命令式语言中叫“变量声明”，本质上两者都是分配内存空间给变量。

语句 statement，表达式 expression，一切都是表达式，表达式返回值。

```ocaml
let greeting = "hello!"
let score = 10
let newScore = 10 + score
```

### 块作用域

let绑定只能在对应的块作用域中生效，跟变量的作用域概念一样。

代码块最后一行的表达式值，会自动被返回，

函数也是代码块，这样就不用明确的return来返回。

```ocaml
let message = {
  let part1 = "hello"
  let part2 = "jack"
  part1 ++ " " ++ part2
}
Js.log(message)
//Js.log(part1) //会报错，在代码块之外访问
```

### 默认不可变

let绑定默认是不可变的(immutable)，这个特性有助于类型系统推断和编译器优化。

默认不可变怎么不是强制的,而仅仅只是建议呢，奇怪，这应该直接编译器报错：

```ocaml
let result = "hello"
Js.log(result) // prints "hello"
let result = 1
Js.log(result) // prints 1
```

### 覆盖绑定

编译器允许let绑定是可以覆盖之前的，覆盖后，之前的绑定失效，新的绑定生效。

从绑定的角度来理解是合理的，但从结果上跟js的动态类型效果是一样的，有点让人误解。

但是rescript仍然是静态类型，任何时刻每一个值都有明确的类型。

```ocaml
let x = 1
Js.log(x)
let x = "abc" //覆盖之前定义的let绑定,编译器是允许的,不过正常情况不要这么使用
Js.log(x)
```

### 私有的绑定

在模块系统中，任何元素都是默认公共的，唯一的方式就是提供一个单独的接口文件resi来列出需要公共的元素，其他元素就是模块私有的。

但是也可以通过%%private直接在res文件中将某个绑定声明为私有：

```ocaml
module Mymodule = {
  %%private(let a = 1) //变模块内私有了
  //   let a = 1  //默认都是公开的
  let b = 2
}

// Js.log(Mymodule.a)   //报错,找不到Mydoule.a
Js.log(Mymodule.b)

```

%%private在下面2个场景中还是比较有用的：

1. 代码自动生成
2. 快速原型

### 变量可变性

```ocaml
let myValue = ref(5) //可变的let绑定,其实绑定还是不变,绑定的是一个引用的record
let five = myValue.contents
myValue.contents = 6 //改变变量的值,干嘛不用val或value，更简洁，更好理解一些

myValue := 7 //改变变量的值，语法糖
Js.log(myValue)
```

真是丑陋的语法形式，干嘛不用mut作为关键字就好了，翻阅了一些ocaml的语法，这个ref的是ocamla中设计的，rescript只是保留不变，其实应该考虑改进一下的，不要受这种坏包袱的影响：

```ocaml
let mut myValue = 5
myValue = 6 
```

其实，关于变量的可变，还可以使用let绑定的同名覆盖，其实也是实现了变量的可变。

```ocaml
let x = 1
Js.log(x)
let x = 2 //覆盖之前定义的let绑定,编译器是允许的
Js.log(x)
```
