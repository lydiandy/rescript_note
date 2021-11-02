## 变体类型

### 类型定义

变体varient：可以理解为变化的类型或联合类型sumtype，表示类型可以是这个类型或那个类型 。

枚举enum是变体的最简单用法，所以在rescrpt里没有专门的枚举类型。

变体类型的子类型要首字母大写

```ocaml
type animal = Dog | Cat | Bird //同一行的写法

type animal2 =
 	| Dog  //不同一行的写法,第一项要多一个|
 	| Cat
 	| Bird 

let a = Dog
let b: animal = Cat

Js.log(a)  // 0
Js.log(b)  // 1
```

### 构造器参数

变体类型的每一项,就是一个类型

类型如果不带参数,就等价于枚举的效果

类型如果带参数,可以理解为类型的构造器函数

构造器函数可以带不同的参数,也可以带一个inline record作为参数

```ocaml
type account =
  | None 
  | Instagram(string) //把变体类型的每一项,理解为一个类型,变体类型的构造器函数可以带不同的参数
  | Facebook(string, int)

let my_account = Facebook("tom", 22)
let my_account2 = Instagram("tom")

Js.log(my_account) // { TAG: 1, _0: 'tom', _1: 22 } //参数是没有名称的
Js.log(my_account2) // { TAG: 0, _0: 'tom' } 

```

变体类型的构造函数参数如果比较多,可以使用一个记录来作为参数,就可以实现有名称的参数。

```ocaml
type user =
  | Number(int)
  | Id({name: string, password: string})

let me = Id({name: "Joe", password: "123"})

Js.log(me) //{ TAG: 1, name: 'Joe', password: '123' } //有名称的参数
```

等价于：

```ocaml
type u = {name: string, password: string}
type user =
  | Number(int)
  | Id(u)

let me = Id({name: "Joe", password: "123"})
```

变体类型中如果是基本类型，不能直接使用基本类型，要使用类型封装类，因为变体类型必须要有构造器函数，这个要求一点都不好，违反直觉。

```ocaml
//错误的代码:
// type myType = bool | int | float
// let t:myType = 1
// Js.log(t)

//正确的代码:
type myType = Bool(bool) | Int(int) | Float(float)
let t: myType = Int(1)
Js.log(t)
```

### 变体与模式匹配

使用变体类型加模式匹配，用来替换else if，增加代码的可读性：

js代码：

```javascript
let data = 'dog'
if (data === 'dog') {
  ...
} else if (data === 'cat') {
  ...
} else if (data === 'bird') {
  ...
}
```

rescript代码：

```ocaml
type animal = Dog | Cat | Bird
let data = Dog
switch data {
| Dog => Js.log("Wof")
| Cat => Js.log("Meow")
| Bird => Js.log("Kashiiin")
}
```

## 多态变体类型

多态变体跟普通变体有以下三个不同：

- 变体中的类型必须以#开头，并且构造器名字不需要大写开头。

- 不用预先有类型定义，类型可以自动推断。

- 不同多态变体类型的值可以共享使用构造器。简单讲就是不同的多态变体类型可以组合使用。

### 创建

不用预先定义类型，直接创建多态变体类型的变量：

```ocaml
let myColor = #red
let myLabel = #good
let myNumber = #3

Js.log(myColor) //返回:red
Js.log(myLabel) //返回:good
Js.log(myNumber) //返回:3

```

### 类型定义

虽然多态变体类型定义是可选的，但是可以预定义一个多态变体类型：

```ocaml
type color = [#red | #green | #blue]
let c: color = #red
//let cc: color = #rrr //报错
Js.log(c)
```

### 行内定义

还可以直接使用行内定义来定义多态变体类型：

```ocaml
let render = (myColor: [#red | #green | #blue]) => { //行内多态变体类型
  switch myColor {
  | #blue => Js.log("Hello blue!")
  | #red
  | #green => Js.log("Hello other colors")
  }
}
render(#blue)
```

### 使用构造器参数

也可以跟普通的变体类型一样，使用构造器参数来定义类型：

```ocaml
type account = [
  | #Anonymous
  | #Instagram(string)
  | #Facebook(string, int)
]

let me: account = #Instagram("Jenny")
let him: account = #Facebook("Josh", 26)
```

### 组合和模式匹配

这点是跟普通多态变体最大的区别。

一般来说，不同的枚举的枚举项是不能共享的，因为表示的含义可能完全不一样。

但是多态变体却可以彼此共享，组合各自的类型

```ocaml
type red = [#Ruby | #Redwood | #Rust]
type blue = [#Sapphire | #Neon | #Navy]

//组合其他多态变体,创建新的,等于这几个多态变体的值可以组合,共享各自的类型
type color = [red | blue | #Papayawhip]

let myColor: color = #Ruby
Js.log(myColor)
```

如果有存在重复的类型，也不会报错，应该是取并集：

```ocaml
type red = [#Ruby | #Redwood | #Navy]
type blue = [#Sapphire | #Neon | #Navy] //blue也跟red重复了

//组合其他多态变体,创建新的,等于这几个多态变体的值可以组合,共享
type color = [red | blue | #Navy] //跟blue的重复了

let myColor: color = #Navy
Js.log(myColor)
```

在模式匹配的时候，匹配的类型超出了多态变体的子类型，也不会报错：

```ocaml
type preferredColors = [#white | #blue]

let myColor: preferredColors = #blue

let displayColor = v => {
  switch v {
  | #red => "Hello red" 	//超出了，多态变体没有这个子类型
  | #green => "Hello green" //超出了，多态变体没有这个子类型
  | #white => "Hey white!"
  | #blue => "Hey blue!"
  }
}

Js.log(displayColor(myColor))

```

### 额外约束

之前提到的多态变体都是闭合的(closed)，还可以是开放的(open)：

```ocaml
// 闭合的:只能是多态变体中其中的一个,其他的不行
let basic: [#Red] = #Red

//开放的: 大于号表示可以是多态变体中其中的一个,也可以是其他的
let foreground: [> #Red] = #Green

//开放的: 小于号表示只能是多态变体中其中的一个,其他的不行
let background: [< #Red | #Blue] = #Red
```

### 多态变体类型转换

可以使用:>，把多态变体类型转换为string或int，转换是零成本的。

```ocaml
type company = [#Apple | #Facebook]
let theCompany: company = #Apple

let message = "Hello " ++ (theCompany :> string)
Js.log(message)
```
