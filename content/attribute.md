## 注解 attribute

js叫装饰器(decorator)

注解必须以@开头

### 类型注解

```ocaml
//类型注解
@unboxed
type a = Name(string)

type student = {
  age: int,
  @as("aria-label") ariaLabel: string, //字段注解
}
```

### 函数注解

```ocaml
//函数注解,注解函数作废
@deprecated
let customDouble = foo => foo * 2

//注解函数作废,并携带一个string
@deprecated("Use SomeOther.customTriple instead")
let customTriple = foo => foo * 3
```

### 外部语句注解

```ocaml
@val external message: string = "message" //外部语句注解
```

### 其他注解

```ocaml
@@warning("-27") //@@开头的是全局注解,给整个文件注解
```







