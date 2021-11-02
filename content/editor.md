## 开发工具

### rescript-vscode插件

https://rescript-lang.org/docs/manual/latest/editor-plugins

首选vscode

rescript官方插件: rescript-vscode

https://github.com/rescript-lang/rescript-vscode

### ocaml官方插件

OCaml Platform: https://marketplace.visualstudio.com/items?itemName=ocamllabs.ocaml-platform

下载插件后，还需要安装lsp：

```shell
opam install ocaml-lsp-server
```

### code runner插件

https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner

```json
"code-runner.executorMap": {
   "rescript":"cd $workspaceRoot && rescript && node $dir$fileNameWithoutExt.bs.js"
 },
```

### 文件后缀

实现文件(implementation)和接口文件(interface)

OCaml: .ml, mli

Reason: .re, .rei

ReScript: .res, .resi 

