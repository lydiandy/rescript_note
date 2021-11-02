## 构建系统

rescript本身是以一个npm包发布的，一般在项目的依赖中：

package.json

```json
  "dependencies": {
    "@ryyppy/rescript-promise": "^2.1.0",
    "rescript": "*"
  }
```

命令行工具路径为：node_modules/rescript/rescript

命令行选项：

```shell
rescript -help
```

```shell
> rescript -help                                      
Available flags
-v, -version  display version number
-h, -help     display help 
Subcommands:
    build    
    clean
    format
    convert
    dump
    help
Run rescript subcommand -h for more details,
For example:
    rescript build -h
    rescript format -h
The default `rescript` is equivalent to `rescript build` subcommand
```

```shell
rescript init				//在当前目录，初始化一个新的项目
rescript init xxx 	//初始化一个新的项目

rescript		//在项目根目录中执行rescript，默认就是编译当前项目

rescript build -w		//编译项目，并保持监听

rescript clean		//清理项目，把编译生成的js文件清除掉

rescript clean -with-deps //同时把依赖生成的文件也清除掉

rescript format 	//格式化当前目录：.res ，.resi，.ml，.mli，.re，.rei

rescript format -all //格式化当前项目的所有源文件

rescript convert	//把reason代码转化为rescript代码

rescript dump  xxx.cmi	//从res构建二进制接口文件(.cmi)并且解析成可阅读的接口
```

## 项目配置文件

bsconfig.json是rescript的项目构建系统配置文件，rescript根据这个配置来进行构建。

bs应该就是build system构建系统的缩写，这会让人误解为是buckle script的缩写，为什么改名为rescript了，这个配置文件还不改为rsconfig.json。

```json
{
  "name": "rescript-project-template",
  "version": "0.0.1",
  "sources": {
    "dir" : "src", 			//rescript源代码的目录
    "subdirs" : true 		//是否包含子目录
  },
  "package-specs": {
    "module": "es6", 		//选项有：commonjs,es6,es6-global
    "in-source": true 	//true直接在res源文件旁生成js,false所有js统一生成到lib目录中
  },
  "suffix": ".mjs", 		//如果是es6或es6-global，一定要用mjs
  "bs-dependencies": [
    "@ryyppy/rescript-promise"
  ],
  "warnings": {
    "error" : "+101"
  }
}

```













