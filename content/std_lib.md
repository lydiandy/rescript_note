## 标准库

标准库文档：https://rescript-lang.org/docs/manual/latest/api

标准库源代码：https://github.com/rescript-lang/rescript-compiler/tree/master/lib

rescritp标准库包含了3个模块：

Js模块：绑定标准js-api

Belt模块：rescript标准库。除了标准js-api以外，额外增加数据结构
	Belt标准库目标：

- 提供高质量的不可变数据结构
- 默认安全的，函数从不抛出异常，除非函数名以Exn后缀
- 更好的性能和更小的代码大小
- 为tree-shaking做准备

Dom模块：提供一个标准的dom类型集合，给第三方库使用。只提供类型，没有函数。

### 使用规则

- 默认使用Js模块，大部分的标准js-api都会在编译后直接调用宿主的api
- Js模块中优先使用Js.Xxx2的模块，不带数字的是旧版的
- Js模块没有的，再去使用Belt中的api

### Js标准库





### Belt标准库





### Node标准库

nodejs的标准模块绑定

Node.Path

Node.Fs

Node.Process

Node.Buffer

Node.Child_process
