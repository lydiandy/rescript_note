## 关键字

使用中的关键字：

```ocaml
and
as
assert
catch
else
exception
external
false
for
if
in
include
lazy
let
module
mutable
of
open
rec
switch
true
try
type
while
```

预留的关键字：

```ocaml
constraint
when
with
```

rescript语言中的关键字没有struct，class，return，fn/function，const，enum，interface 确实比较特别：

- struct,class,enum都在type中

- 变量默认不可变，const没有存在的必要

- fn/function都用胖箭头函数来表示了

- 函数的最后一个表达式的值，会被自动返回，所以return也没有必要的

  

- 接口确实没有

