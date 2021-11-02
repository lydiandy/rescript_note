## 选项类型 option

rescript中没有null和undefined的概念，这是个很棒的事，可以去除掉一整类的bug。

通过内置option类型来处理是否空值的情况。

内置option类型，是变体类型和泛型的组合使用：

```ocaml
type option<'a> = None | Some('a)
```

None: 表示什么都没有，nothing，也就是空值，

Some('a): 表示有某个东西，something，那个东西就是'a类型的值。

```ocaml
let personHasACar = true
let licenseNumber = if personHasACar {
  Some(5)
} else {
  None
}

Js.log(licenseNumber) //5
```

等价的js代码：

```js
var licenseNumber = personHasACar ? 5 : undefined
```

然后可以使用模式匹配：

```ocaml
switch licenseNumber {
| None =>
  Js.log("The person doesn't have a car")
| Some(number) =>
  Js.log("The person's license number is " ++ 			   Js.Int.toString(number))
}
```

