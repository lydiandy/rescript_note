## 惰性求值 lazy value

延迟执行并缓存起来(defer and cache)

关键是不用每次都执行,会缓存起来,性能更好

### 定义lazy代码块

```js
let expensiveFilesRead = lazy {
  //使用内置的lazy函数包装一个要延迟执行的代码块
  Js.log("Reading dir")
  Node.Fs.readdirSync(".") //读取当前目录的内容,返回数组
}
```

### 触发真正执行

再次触发执行后,编译器就会自动判断缓存是否生效,如果生效,则直接使用缓存结果,不再去重新一遍

```js
//调用Lazy模块的force函数,第一次调用,真正执行
Js.log(Lazy.force(expensiveFilesRead))

//第二次调用,直接使用缓存结果返回
Js.log(Lazy.force(expensiveFilesRead))
```

不需要使用Lazy.force,使用模式匹配：

```js
switch expensiveFilesRead {
| lazy result => Js.log(result)
}
```

不需要使用Lazy.force,直接使用let绑定:

```js
let lazy result = expensiveFilesRead
Js.log(result)
```

结合错误处理：

```js
//使用try catch进行异常处理
let result = try {
  Lazy.force(expensiveFilesRead)
} catch {
| Not_found => [] //比如文件找不到了,就返回空数组,作为结果
}
```



