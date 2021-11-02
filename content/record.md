## 记录

record(记录)就是静态类型语言中的struct(结构体)，跟js中的object(对象)不一样，有以下2个不同：

- 默认不可变

- 字段固定，不可扩展

在ocaml中，record对应C中的struct(结构体)；variant对应C中的union(联合体)。

### 类型声明

```ocaml
type person = { 	//类名是小写的
  age: int,
  name: string,
}

let me = {	//类型推断为person
  age: 5,
  name: "jact",
}
Js.log(me)

let tom: person = {age: 10, name: "tom"} //明确类型
Js.log(tom)
```

### 可变字段

类型中的字段默认是不可变的，可以使用mutable关键字，变为可变。

```ocaml
type person = {
  mutable age: int,
  name: string,
}
let tom: person = {age: 10, name: "tom"}
tom.age = 12
Js.log(tom)
```

### 基于记录创建新记录

在原有记录的基础上,其他字段不变,只更新有变化的字段

```js
let meNextYear = {...me, age: me.age + 1}
```

## 对象

不需要类型声明，字面量方式创建：

```ocaml
let me = {
  "age": 5, //注意：字段名是带双引号的，如果不带，就会去匹配类型
  "name": "Big ReScript",
}
Js.log(me)
```

### 访问对象字段

```ocaml
let age = me["age"]
Js.log(age)
```

### 类型组合

```ocaml
type point2d = {
  "x": float,
  "y": float,
}
type point3d = {
  ...point2d,
  "z": float,
}

let myPoint: point3d = {
  "x": 1.0,
  "y": 2.0,
  "z": 3.0,
}
Js.log(myPoint)
```
