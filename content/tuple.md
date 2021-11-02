## 元组

元组是一个特别的数据结构，js里并不存在，有以下几个特性：

- 不可变

- 有顺序

- 创建时大小就固定下来

- 可以同时包含不同类型的值

```ocaml
let age_and_name = (24, "jack", "good boy")
let age_and_name2: (int, string, string) = (24, "jack", "good")
Js.log(age_and_name)
Js.log(age_and_name2)
```

### 元组的解构赋值

就像js的解构赋值那样：

```ocaml
let age_and_name = (24, "jack", "good boy")
let (age, name, _) = age_and_name
```

### 默认不可变

如果要修改元组中元素的值，那就只能，先解构，然后再创建一个新的元组变量

```ocaml
let coordinates1 = (10, 20, 30)
let (c1x, _, _) = coordinates1
let coordinates2 = (c1x + 50, 20, 30)
```

### 元组作为函数的返回值

把元组作为函数的返回值，就实现了函数的多返回特性。

```ocaml
let get_number = (x, y) => {
  (x, y)
}
Js.log(get_number(1, 3))
Js.log(get_number("a", "b"))
```

完整函数签名版本：

```ocaml
let get_number = (x: int, y: int): (int, int) => {
  (x, y)
}
Js.log(get_number(1, 3))
```





















