##js互操作 (js interop)

2种技术实现方式：注解和扩展点，注解是主要的方式。

### 嵌入原始的s代码 raw

执行原始的js代码段

```ocaml
let add = %raw("(a,b) => a+b") //js表达式
%%raw("const a = 12") //单独的语句

let code = "console.log('abc')"  
//%%raw(code) //会报错,raw只接受字符串,不能接受任何变量,估计是怕被注入

let sum = add(1, 3)
Js.log(sum)

//使用反引号，来保存多行的原始js代码
%%raw(`
// look ma, regular JavaScript!
var message = "hello";
function greet(m) {
  console.log(m)
}
`)
```

###绑定外部js代码 external

把js函数或对象引入到rescript中，实现在rescript中使用js函数或对象。       

结合external使用，跟let绑定类似，

let绑定：把名字跟某个值，表达式，函数，绑定在一起。

external绑定：把名字跟某个外部的js函数或对象绑定在一起。              

绑定好以后，就可以像rescript的函数或变量那样，使用外部js的函数或对象。                                                                                                                                                                                                                                                                         

```ocaml
@val 
external setTimeout: (unit => unit, int) => float = "setTimeout"

let say = () => {
  Js.log("abc")
}

let res = setTimeout(say, 1000)

@val @scope("Math") 
external random: unit => float = "random"

let someNumber = random()
Js.log(someNumber)

@val @scope(("window", "location", "ancestorOrigins")) 
external length: int = "length"

//scope感觉是多余的，直接这样不更简单，直观：
//@val
//external length: int = "window.location.ancestorOrigins.length"

let l = length
```

### 绑定js对象

如果要绑定的js对象有固定的字段，就可以使用记录来实现绑定：

```ocaml
type person = {
  name: string,
  friends: array<string>,
  age: int,
}

@module("MySchool")
external john: person = "john"

let johnName = john.name
let johnName = john["name"]
```

如果需要给对象的字段取别名，可以使用@as：

```ocaml
type action = {
  @as("type") 
  type_: string
}
```

如果要绑定的js对象字段不是固定的，可能是动态增减字段的，那就需要使用Js.Dict来绑定

绑定的对象是一个js内置的构造函数：

```ocaml
type t
@new 
external createDate: unit => t = "Date"

let date = createDate()
```

如果js的构造函数是从某个模块导入而来：

```ocaml
type t
@new @module 
external book: unit => t = "Book"
let myBook = book()
```

### 绑定js函数

绑定js函数就是external的标准用法

```ocaml
@module("path") 
external dirname: string => string = "dirname"
let root = dirname("/User/github") 
```

如果要绑定的js函数有可能有存在可选参数的情况，也可以用rescript的可选参数来支持：

```js
// MyGame.js

function draw(x, y, border) {
   // suppose `border` is optional and defaults to false
}
draw(10, 20)
draw(20, 20, true)
```

```ocaml
@module("MyGame")
external draw: (~x: int, ~y: int, ~border: bool=?, unit) => unit = "draw"

draw(~x=10, ~y=20, ~border=true, ())
draw(~x=10, ~y=20, ())
```

### 绑定js对象的方法

rescript的函数式部分没有方法的概念，要绑定js对象的方法，就是把方法转变为函数，对象是函数的第一个参数：

```ocaml
type document
@val 
external doc: document = "document"
@send 
external getElementById: (document, string) => Dom.element = "getElementById"

let el = getElementById(doc, "myId")

```

### 









