## 管道

rescript提供了一个很小却超级有用的操作符：管道操作符->，

可以把代码从 b(a) 变为 a -> b, 这只是一个简单的语法糖,没有任何运行时成本，却让代码变得更符合直观,从左到右。

### 单层调用

直观,简单,只要是函数就可以：

```ocaml
parseData(person)
//变为
person->parseData
```

如果函数有多个参数也可以使用，作为第一个参数传入：

```ocaml
a(one,two,three)
one->a(two,three)		//作为第一个参数传入
```

因为rescript是以函数式为主，只有函数，没有方法，使用管道运算符可以让代码有调用方法的感觉，很直观。

```ocaml
open Js.Array2
let myArray = [1, 3, 5, 7]
Js.log(length(myArray)) //只有函数调用,只能把数组作为函数的第一个参数来模拟方法,不直观
Js.log(myArray->length) //可以使用array->length,就跟面向对象语言的array.length的效果一样
```

### 多重嵌套调用

```ocaml
validateAge(getAge(parseData(person)))
//使用管道变为
person->parseData->getAge->validateAge
//或者
person
  ->parseData
  ->getAge
  ->validateAge
```

在面向对象语言中，比如js，有类似效果的只有链式调用，但链式调用必须把函数变为方法，所有方法的返回值必须返回对象自身，没有管道这么自由，只要是普通的函数就可以。

### 管道结合变体

```ocaml
//普通写法
let result = Some(preprocess(name))
//管道写法
let result = name->preprocess->Some
```









