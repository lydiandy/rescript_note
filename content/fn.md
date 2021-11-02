## 函数

### 函数定义

没有函数关键字,只有胖箭头,只有这种唯一的函数定义形式。

跟变量定义一样，也是通过let绑定来定义函数,挺统一的，函数也是一种类型。

```js
let greet = (name) => "Hello " ++ name
```

如果没有参数,使用空括号：

```js
let greetMore = () => {...}
```

函数的参数，返回值的类型推断：

```ocaml
let greet = name => {
  "hello" ++ name
}
Js.log(greet("tom"))


let add = (x, y, z) => { //看似是动态类型
  x + y + z //通过加号运算符，推断是整数类型
}

let add2 = (x: int, y: int, z: int) => { //看似没有返回值，也不知道类型
  x + y + z	//代码块的最后一行的表达式自动会被返回，加法运算符也推断返回值是整数类型
}

let add2 = (x: int, y: int, z: int): int => { //也可以显示标识参数和返回的类型
  x + y + z
}

Js.log(add(1, 2, 3))
Js.log(add2(1, 2, 3))
```

也可以手工把参数和返回值的类型声明出来，不用编译器类型推断，让函数签名很明确：

```ocaml
let say = name => "hello" ++ " " ++ name

let say2 = (name: string): string => "hello" ++ " " ++ name

let say3 = (name: string): string => {
  "hello" ++ " " ++ name
}

Js.log(say("jack"))
Js.log(say2("tom"))
Js.log(say3("mary"))

```

### 命名参数

命名参数就是给参数进行命名，传递参数时，只跟参数的名字来匹配，不跟参数的顺序：

```ocaml
let sub = (~x, ~y) => {
  x - y
}

Js.log(sub(~x=3, ~y=1))
Js.log(sub(~y=1, ~x=3))

```

其实如果命名参数不要~，也没有什么问题，看着更简洁，目前不行。

参数前面加 ~也是从ocaml的语法保留过来的。

```ocaml
let sub = (x, y) => {
  x - y
}

Js.log(sub(x=3, y=1))
Js.log(sub(y=1, x=3))

```

### 参数取别名

还可以对参数进行as取别名，在函数内部使用

```ocaml
let sub = (~width as w, ~y) => { //取别名width变为w，函数内width不能使用，只能使用w
  w - y
}

Js.log(sub(~width=3, ~y=1))
Js.log(sub(~y=1, ~width=3))

```

### 可选命名参数

```ocaml
let drawCircle = (~color, ~radius=?, ()) => {
  setColor(color)
  switch radius {
  | None => startAt(1, 1) //radius没有传进来，就是None
  | Some(radius) => startAt(radius, radius) //radius有传递参数进来，就是Some()
  }
}
```

加上完整类型标注的写法，更清晰，也更冗长：

```ocaml
let drawCircle: (~color: color, ~radius: int=?, unit) => unit =
  (~color: color, ~radius: option<int>=?, ()) => {
    setColor(color)
    switch radius {
    | None => startAt(1, 1)
    | Some(r_) => startAt(r_, r_)
    }
  }
```

### 参数默认值

```ocaml
let drawCircle = (~radius=1, ~color, ()) => {
  setColor(color)
  startAt(radius, radius)
}
```

### 递归函数

使用rec来特别标识有函数递归调用，rec是recursive递归的缩写。

```ocaml
let rec neverTerminate = () => neverTerminate()
```

互相递归调用：

```ocaml
let rec callSecond = () => callFirst()
and callFirst = () => callSecond()
```

### 多个返回值

因为有元组类型，所以可以直接返回元组，就是多个返回值：

```ocaml
let add = (x, y, z) => (x, y, z) 	//返回元组，就是返回多个值

let (a, b, c) = add(1, 2, 3)	//然后使用元组的解构赋值，实现一次接收多个返回值

Js.log3(a, b, c)

```

### 函数类型

函数类型的简单解释就是：这一类函数，相同函数签名的函数。

可以作为函数参数的类型，把满足函数类型的函数作为参数或返回值传递。

```ocaml
type add = (int, int) => int
type add2 = (~first: int, ~second: int) => int //带命名参数
type add3 = (~first: int=?, ~second: int=?, unit) => int //带命名参数和可选参数
```

### 不确定个数参数

目前没有看到这个特性，比如：

```ocaml
let add = (x,y,...z) => {
    let sum = ref(0)
    for i in z {
        sum := sum+i
    }
    x+y+sum
}
```

