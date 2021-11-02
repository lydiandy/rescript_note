## 模块

模块就像是最小的文件，模块可以包含类型定义，let绑定，以及嵌套模块。

一个res文件就是一个模块，但不是最小的，res文件中还可以再包含模块，模块可以多个层级嵌套，就是命名空间的用途。

### 定义模块

使用module来定义模块，模块名强制大写开头，大驼峰风格。

```ocaml
//模块
module School = {
  //类型
  type profession = Teacher | Director
  //变量绑定
  let person1 = Teacher
  //函数
  let getProfession = person => {
    switch person {
    | Teacher => "A teacher"
    | Director => "A director"
    }
  }
}

let p = School.Director
Js.log(School.getProfession(p))
```

模块嵌套：

```ocaml
module MyModule = {
  module NestedModule = {
    let message = "hello"
  }
}

let message = MyModule.NestedModule.message
Js.log(message)
```

### 导出

默认res代码中的所有类型声明，变量绑定，函数声明等都是公共的，其他模块可以直接使用。

如果只需要导出一部分，那就需要定义resi接口文件，把公共的部分声明出来。

其他没有在接口文件中声明为公共的就变为模块内部使用的。

### 导入

rescript不需要导入语句，会在项目中所有目录和子目录中查找该模块。	

项目中的res文件名要保持唯一，不仅仅是同级目录唯一，项目中的所有目录和子目录都要唯一。

模块名必须是大写字母开头，所以目录名最好也一样大写字母开头。目录层级不要太多，文件名用下划线来区分不同的层级，以保持文件名的唯一。

### 打开模块

打开模块可以用来省略模块前缀

```ocaml
let p = School.getProfession(School.person1)
```

```ocaml
open School
let p = getProfession(person1)
```

### 模块别名

如果模块有多层级嵌套，使用的时候可以全路径写，也可以通过定义一个模块别名，来缩短全路径写。

这个特性目前主要在React中用到。

Button模块中嵌套了一个Label子模块：

```jsx
// src/Button.res
module Label = {
  @react.component
  let make = (~title: string) => {
    <div className="myLabel"> {React.string(title)} </div>
  }
}

@react.component
let make = (~children) => {
  <div>
    <Label title="Getting Started" />
    children
  </div>
}
```

全路径使用Label模块是这样的：

```jsx
<Button.Label title="My Button"/>
```

使用模块别名，来缩短引用的路径：

```ocaml
module Label = Button.Label
let content = <Label title="Test"/>
```

### 模块签名(模块类型)

模块就像是实现文件res，模块签名就像接口文件resi，也可以理解为模块类型，某一类满足条件的模块。

模块签名就像接口的概念那样，而实现模块就像类那样，模块实现要满足模块签名的要求。

分清楚：普通模块(模块实现)，模块签名(模块类型)

```ocaml
//模块签名，可以把模块签名当做一个接口
module type MyModule = {
  let x: int //要求模块有一个let绑定,名字为x,类型为int
  type t = string //要求模块中有一个字段t,类型是string
  type f //要求模块中有一个字段f,但是没有要求字段的类型
}

module type EstablishmentType = {
  type profession //要求模块有一个类型叫profession
  let getProfession: profession => string //要求模块有一个函数，参数为professiong,返回string
}
```

### 模块函数functors

模块可以传递给函数

- 模块函数的定义要用module，而不是let。
- 模块函数把模块作为参数，把模块作为返回值。
- 模块函数必须大写字母开头，就跟模块一样。

```ocaml
//模块签名/模块类型
module type Comparable = {
  type t
  let equal: (t, t) => bool
}

//模块函数,把Comparable模块类型作为参数,满足这个类型的才可以作为参数传入
module MakeSet = (Item: Comparable) => {
  // let's use a list as our naive backing data structure
  type backingType = list<Item.t>
  let empty = list{}
  let add = (currentSet: backingType, newItem: Item.t): backingType =>
    // if item exists
    if List.exists(x => Item.equal(x, newItem), currentSet) {
      currentSet // return the same (immutable) set (a list really)
    } else {
      list{newItem, ...currentSet} // prepend to the set and return it
    }
}

//具体的实现模块,满足Comparable模块类型的要求,作为模块函数的参数
module IntPair = {
  type t = (int, int)
  let equal = ((x1: int, y1: int), (x2, y2)) => x1 == x2 && y1 == y2
  let create = (x, y) => (x, y)
}

module SetOfIntPairs = MakeSet(IntPair) //模块函数返回后,得到一个具体的模块

```

### 模块函数类型

模块函数类型，是模块函数的类型。

分清楚：普通函数，模块函数，模块函数类型

```ocaml
module type Comparable = ...

module type MakeSetType = (Item: Comparable) => {
  type backingType
  let empty: backingType
  let add: (backingType, Item.t) => backingType
}

module MakeSet: MakeSetType = (Item: Comparable) => {
  ...
}
```

### 包含模块

include

类似模块继承或mixin

这个就像是编译器级别的赋值黏贴,把模块的内容黏贴到包含的模块中

包含了模块后,还可以覆盖重写模块原有的函数或变量

不鼓励使用包含模块的功能,作为最后的方案

```ocaml
module BaseComponent = {
  let defaultGreeting = "Hello"
  let getAudience = (~excited) => excited ? "world!" : "world"
}

module ActualComponent = {
  include BaseComponent //包含模块
  
  //包含了模块后,还可以覆盖重写 BaseComponent.defaultGreeting 
  let defaultGreeting = "Hey"
  
  //直接使用模块中的函数
  let render = () => defaultGreeting ++ " " ++ getAudience(~excited=true) 
}
```

### 模块搜索路径

1. 搜索项目当前目录及子目录。
2. 根据bsconfig.jsong中设置的依赖模块，搜索node_modules中的第三方目录。