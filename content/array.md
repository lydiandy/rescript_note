## 数组

数组是最主要的有序数据结构，就跟js中的数组一样：可以访问，修改，动态改变大小。

跟js不一样的是rescript数组的元素必须是同类型的。

```ocaml
let myArray = ["a", "b", "c", "d"]
Js.log(myArray)
let firstItem = myArray[0] //访问数组元素
myArray[0] = "aa" //修改数组元素
Js.log(myArray)
let pushedValue = Js.Array2.push(myArray, "e") //追加数组元素
Js.log(myArray)
```

所有数组操作的函数都在Js.Array2和Belt.Array模块中。

## 列表

列表跟数组一样,元素必须是同类型，但是又有以下区别：

- 默认不可变

- 往列表头部插入数据很快

- 访问列表尾部数据很快

- 其他操作都比列表慢

如果你需要的是随机访问数组,或者不是在表头插入数据，就不要使用列表，速度没有数组快。

```ocaml
let mylist = list{1, 2, 4}

Js.log(mylist) //{ hd: 1, tl: { hd: 2, tl: { hd: 4, tl: 0 } } }

```

列表插入数据

```ocaml
let myList = list{1, 2, 3}
let anotherList = list{0, ...myList} //使用展开运算符，在mylist的基础上，在头部插入数据

Js.log(myList)	//list{1,2,3}
Js.log(anotherList) //list{0, 1, 2, 3}

```





