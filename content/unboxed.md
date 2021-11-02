## 拆箱 unboxed

针对单个字段的类型，单个参数的变体类型。

为了性能考虑和某些js集成的场景，rescript提供了一种方式叫unboxed。

unboxed/unwrap，也叫拆箱/拆开，

就是标注@unboxed以后,把**单个字段的记录**或**单个参数的变体**，拆开变为基本类型。

如果没有标注生成的就是js的对象：

```ocaml
//标注变体类型name,要进行拆箱
@unboxed
type name = Name(string)

let studentName = Name("Joe")
let studentName2 = Name("tom")

//标注普通类型greeting,要进行拆箱
@unboxed
type greeting = {message: string}

let hi = {message: "hello!"}
let hi2 = {message: "hello2"}

```

生成没有拆箱的js代码:

```js
var studentName = /* Name */{
  _0: "Joe"
};
var studentName2 = /* Name */{
  _0: "tom"
};
var hi = {
  message: "hello!"
};
var hi2 = {
  message: "hello2"
};
```

生成拆箱后的js代码:

```js
var studentName = "Joe";
var studentName2 = "tom";
var hi = "hello!";
var hi2 = "hello2";
```



