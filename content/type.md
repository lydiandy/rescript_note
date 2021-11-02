## 类型系统

类型是rescript的亮点：

- 健壮

  变量的类型一旦确定，就不能变成另一种类型。

- 静态

  rescript的类型信息编译成js代码时会被擦除，不会在运行时存在。不用担心类型会拖慢代码的运行速度，运行时是不需要类型信息的。而编译时，类型信息有助于更早地捕捉错误。

- 完整

  完善的类型系统，确保永不出错。大部分类型系统需要去猜测变量的类型，可能会出错，rescript从不需要去猜测，编译器可以准确地判断出变量的类型。

- 快速

  因为变量类型信息可以很准确地被识别出来，所以类型检查的速度是非常快的，编译速度也就很快。

- 易于推断

  可以不需要人为地逐个变量去标明类型，rescript能够从变量的值准确地推断变量的类型，又快又准。

### 类型推断

代码中可以不需要人为写明变量类型，可以根据变量的值或运算符准确地推断变量的类型：

```ocaml
let score = 10	//10就是整数
let add = (a, b) => a + b	//+运算符是整数专用的，所以就是整数
```

### 类型标注

也可以人为的明确标注类型：

```ocaml
let score: int = 10
```

更多明确的类型标注

```ocaml
let myInt = 5
let myInt2: int = 5 	//变量类型
let myInt3 = (4: int) + (5: int) //表达式中值的类型标注
let add = (x: int, y: int): int => x + y 	//函数的参数和返回值的类型标注

Js.log(myInt)
Js.log(myInt2)
Js.log(myInt3)
Js.log(add(1, 3))
```

### 类型别名

```ocaml
type scoreType = int

let x: scoreType = 3
Js.log(x)

```

### 类型参数（泛型）

把类型当做参数传递，类型参数的名字要以单引号'开头（就其他语言中泛型的的T，U）

未使用类型参数的写法：

```ocaml
type intCoordinates = (int, int, int)
type floatCoordinates = (float, float, float)

let a: intCoordinates = (10, 20, 20)
let b: floatCoordinates = (10.5, 20.5, 20.5)
```

使用类型参数的写法：

元组泛型：

```ocaml
type coordinate<'a> = ('a, 'a, 'a)
let a: coordinate<int> = (1, 2, 3)
let b: coordinate<float> = (1.1, 2.3, 3.5)
Js.log(a)
Js.log(b)
```

数组泛型：

```ocaml
array<‘a’>
```

类型泛型：

```ocaml
type result<'a, 'b> = Ok('a) | Error('b)
```

### 递归类型

类型可以像递归函数那样，自己引用自己

```ocaml
type rec person = {
  name: string,
  friends: array<person>
}
```

互相递归：

```ocaml
type rec student = {taughtBy: teacher}
and teacher = {students: array<student>}
```


