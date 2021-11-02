## 异步代码 async/promise

rescript中异步编程的主要机制跟js一样:callback和promise。

目前还不支持async和await关键字。

目前支持的Promise库：

代码库：https://github.com/ryyppy/rescript-promise

安装：

```shell
npm install @ryyppy/rescript-promise --save
```

然后在bsconfg.json中加入：

```json
{
  "bs-dependencies": ["@ryyppy/rescript-promise"]
}
```

使用说明：https://github.com/ryyppy/rescript-promise

