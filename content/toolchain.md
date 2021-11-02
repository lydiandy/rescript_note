## 工具链-命令行使用



## 构建系统 bsb

rescritp自己的构建系统,快速,简洁

编译的速度超级快,不管是冷启动还是热编译,比weback快太多,跟vite有一拼

跟node的npm差不多,估计是希望以后可以独立存在,像node,deno那样有独立的一个生态,可以用于前端,也可以用于后端,这个估计是今后的发展方向,虽然目前还是重点放在前端

目前rescript自己也是以npm包的方式发布,这样更方便前端开发采用,node是绝对主流的前端工具链

### 创建新项目

```shell
bsb -init my-dir
```

项目配置文件: bsconfig.json

### 查看可用的项目模板

```
bsb -themes
```

目前可用的项目模板:

https://github.com/rescript-lang/rescript-compiler/tree/master/jscomp/bsb/templates

```shell
basic
basic-reason
generator
minimal
node
react-hooks
react-starter
tea
```

查看bsb所有命令说明

```shell
bsb -help
```

```shell
-v            Print version and exit
  -version      Print version and exit
  -verbose      Set the output(from bsb) to be verbose
  -w            Watch mode
  -clean-world  Clean all bs dependencies
  -clean        Clean only current project
  -make-world   Build all dependencies and itself 
  -install      Install public interface files into lib/ocaml
  -init         Init sample project to get started. 
                Note (`bsb -init sample` will create a sample project while 
                `bsb -init .` will reuse current directory)
  -theme        The theme for project initialization. 
                default is basic:
                https://github.com/rescript-lang/rescript-compiler/tree/master/jscomp/bsb/templates
  -themes       List all available themes
  -where        Show where bsb.exe is located
  -ws           [host:]port 
                specify a websocket number (and optionally, a host). 
                When a build finishes, we send a message to that port. 
                For tools that listen on build completion.

```

### 项目npm脚本

```json
  "scripts": {
    "clean": "bsb -clean-world",	//清除编译文件
    "build": "bsb -make-world",   //编译项目
    "watch": "bsb -make-world -w" //编译项目,并监控文件变化,实时重新编译
  }
```

运行编译项目后,会根据bsconfig配置文件编译生成js代码,一般是在lib/js/src目录中

### 项目配置文件 bsconfig

```json
{
  "name": "@rescript/react",
  "reason": { "react-jsx": 3 },
  "sources": [
    {"dir": "src", "subdirs": true},
    { "dir": "test", "type": "dev" }
  ],
  "package-specs": [{ "module": "commonjs", "in-source": true }],
  "suffix": ".bs.js",
  "bs-dev-dependencies": ["reason-test-framework"],
  "bsc-flags": []
}

```

所有配置选项说明:

https://rescript-lang.org/docs/manual/latest/build-configuration-schema



