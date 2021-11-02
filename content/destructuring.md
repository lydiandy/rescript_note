## 解构赋值

可以对大部分的内置数据结构进行解构赋值，元组,数组,列表,记录,对象,变体类型，甚至是模块都可以：

### 元组

```ocaml
let coordinates = (10, 20, 30)
let (x, _, _) = coordinates
Js.log(x) // 10
```

### 数组和列表

```ocaml
let myArray = [1, 2, 3]
let [item1, item2, _] = myArray

let myList = list{1, 2, 3}
let list{head, ...tail} = myList
```

### 记录和对象

```ocaml
type student = {name: string, age: int}
let student1 = {name: "jack", age: 10}
let {name} = student1
Js.log(name)
```

### 模块

模块可像解构赋值记录和对象那样，进行解构赋值：

```ocaml
module User = {
  let user1 = "Anna"
  let user2 = "Franz"
}

// 按名字来解构赋值模块内部的变量
let {user1, user2} = module(User)

// 解构赋值,并取别名
let {user1: anna, user2: franz} = module(User)=
```

### 变体类型

```ocaml
type result = Success(string)
let myResult = Success("done")
let Success(message) = myResult //解构赋值到message变量
Js.log(message) //输出：done
```

### 解构同时改名

```ocaml
let {name: n} = student1 //赋值给n
```

