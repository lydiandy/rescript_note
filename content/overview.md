## 快速总览

核心特性:

- 编译很快
- 运行很快
- 可以生成js,生成简洁,高效的js代码
- 可以生成可执行文件,生成的可执行文件很小
- 类型系统很清晰,强大
- 基于ocaml开发,函数式编程
- 采用js开发者都熟悉的语法
- facebook大厂背后推动
- 跟react生态集成方便

### 定位

作者近期更新：https://zhuanlan.zhihu.com/p/377312046

ReScript有很多先进的理念，我希望它是一个起点，在ReScript 的基础上慢慢迭代衍生出一个超越LLVM的编译器框架

编译器，语言设计这一块有三个特点：

- 做一个工业级的很难，需要很多年的积累。
- 理论上很多年没有什么breakthrough了。
- 对整个软件体系的影响很大，很重要，当你不把它当做黑盒子的时候，结合应用是有很多底层创新的机会的。

### 基本概念及历史

OCaml 意为 Objective Caml，是 Caml 语言的主要实现，支持命令式、函数式和面向对象等多种编程范式。

 Caml 语言（Categorical Abstract Machine Language，范畴论抽象机器语言）。

OCaml 的祖先 ML 语言，全称是 Meta Language，设计之处的种种特性都是为了便于开发其他语言的编译器。OCaml 作为继承者，在开发其他编程语言方面也是硕果累累。

OCaml 的确影响了相当多的当红语言，比如 Facebook 内部使用的支持协程的 PHP 编译器 Hack、苹果推出的吸收了很多 OCaml 语言特点的 Swift 语言、微软 .Net 平台上的 OCaml 方言 F#。甚至如今拥趸众多的 Rust 语言，其完成自举之前的初期版本也是由 OCaml 编写的。

- 1985 年，法国高等师范学院（ENS）发布了 Caml 语言。

- 1996 年，OCaml 发布了第一个版本，其在 Caml 的基础上支持了面向对象编程。（为什么一门函数式语言要加入面向对象的特性呢？要知道 Java 是在 1995 年发布的，当时 Caml 加入 OO 特性可能是为了蹭热点。谁知现在 FP 这么火，几成显学）。

- 2012 年 OCaml 社区发布了包管理软件 Opam。

最初的 Caml 语言被暱称为 Heavy Caml，因为其使用 Lisp 实现，使用了过多的 CPU 和内存。后来 Xavier Leroy 和 Damien Doligez 使用 C 重新编写了 Caml 的编译器，称为 Caml Light。

### 编译 & 构建

OCaml 的编译速度和运行速度都很快，编译器的开发者追求在可行的范围内将速度提升到极致。

OCaml 有多种执行方式，可以解释执行，也可以编译到字节码或者者编译为本地可执行文件，还可以编译为 JavaScript。

将 OCaml 代码编译为字节码运行，需要使用 ocamlc 这个命令。将 OCaml 代码编译为本地可执行文件，需要使用 ocamlopt 这个命令。对于稍大的工程，需要使用 ocamlbuild 进行构建，而严肃的大型项目，通常使用 dune 这个工具进行构建。ocamlbuild 和 dune 并不是 OCaml 发行版中的标准工具，需要使用 opam install 进行安装。

jsofocaml 可以将 OCaml 字节码编译到 Javascript 代码，Bucklescript 则将 OCaml 源码编译到 JavaScript 代码。BuckleScript 生成的代码更轻便，但 jsofocaml 也有其使用场景，这两个工具大家可以选择使用。jsofocaml 需要用 Opam 进行安装，BuckleScript 需要用 NPM 安装。

OCaml 可以编译到本地可执行程序执行，执行效率通常是 Python 和 Ruby 这样的语言的数十倍，而其表达能力又不弱于这两者。
