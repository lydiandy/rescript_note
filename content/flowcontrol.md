## 流程控制

### 条件语句

if是一个表达式，条件不用带括号

```js
let message = if isMorning {
  "Good morning!"
} else {
  "Hello!"
}
```

支持三元运算符:

```js
let message = isMorning ? "Good morning!" : "Hello!"
```

### 循环语句

#### for循环

##### 遍历区间

```ocaml
for i in 1 to 5 { //闭区间
    Js.log(i)
}
```

```ocaml
for i in 10 downto 1 {	//闭区间
  Js.log(i)
}

```

##### 如何遍历：字符串，数组/列表，记录/对象 ？



#### while循环

```js
while testCondition {
  // body here
}
```

where循环没有continue，break，也没有return(跟函数一样没有)。

要提前中断循环，可以通过可变绑定：

```js
let break = ref(false)

while !break.contents {
  if Js.Math.random() > 0.3 {
    break := true
  } else {
    Js.log("Still running")
  }
}
```

## 模式匹配

rescript最好的功能之一就是模式匹配,模式匹配组合了3个绝妙的功能为一个：

- 解构赋值
- switch基于数据的形状
- 穷尽检查

### switch匹配变体类型

针对变体类型,可以进行模式匹配:

```ocaml
type payload =
  | BadResult(int)
  | GoodResult(string)
  | NoResult

let data = GoodResult("Product shipped!")
switch data {
| GoodResult(theMessage) => Js.log("Success! " ++ theMessage)
| BadResult(errorCode) =>
  Js.log("Something's wrong. The error code is: " ++ Js.Int.toString(errorCode))
| NoResult => Js.log("Bah.")
}

```

### 匹配多个变量

```ocaml
type animal = Dog | Cat | Bird
let categoryId = switch (isBig, myAnimal) {
| (true, Dog) => 1
| (true, Cat) => 2
| (true, Bird) => 3
| (false, Dog | Cat) => 4
| (false, Bird) => 5
}
```

### 掉落/穿透匹配

```ocaml
let myStatus = Vacations(10)

switch myStatus {
| Vacations(days)
| Sabbatical(days) => Js.log(`Come back in ${Js.Int.toString(days)} days!`)
| Sick
| Present => Js.log("Hey! How are you?")
}
```

### 忽略参数

```ocaml
switch person1 {
| Teacher(_) => Js.log("Hi teacher")
| Student(_) => Js.log("Hey student")
}
```

### 穷尽可能

```ocaml
switch myStatus {
| Vacations(_) => Js.log("Have fun!")
| _ => Js.log("Ok.") //下划线处理其他未列出的可能情况
}
```

如果没有下划线，编译时会检查是否有忽略的情况，要求穷尽所有可能情况。















