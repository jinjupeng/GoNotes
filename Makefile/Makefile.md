# Go之Makefile指南

## 简介

Make是一个构建自动化工具，会在当前目录下寻找Makefile或makefile文件。如果存在，会根据Makefile的构建规则去完成构建。

## Makefile语法

Make基本格式如下：

```
<target> ... : <prerequisites> ...
[tab]  <commands>
    ...

```

其中：

+ target：一个目标代表一条规则，可以是一个或多个文件名，也可以是某个操作的名字（标签），称为伪目标（phony）
+ prerequisites：前置条件，这一项是可选参数。通常是多个文件名、伪目标。它的作用是 target 是否需要重新构建的标准，如果前置条件不存在或有过更新（文件的最后一次修改时间）则认为 target 需要重新构建
+ command：构建这一个target的具体命令集（任意的shell命令），Makefile中的命令必须以[tab]开头
+ .PHONY：伪目标，执行目标下的命令
+ 不带参数，默认执行第一个target
+ @表示禁止回声（正常条件下，make会打印每条命令，然后再执行，就是回声echoing），即终端不会打印真实的执行命令
+ #表示注释
+ ${val}表示变量，和shell脚本中的变量的声明和使用一致
+ 允许使用通配符

比如我们平时使用的 gcc a.c b.c -o test 这里的 test 就是我们要生成的目标， a.c、b.c就是我们生成目标需要的依赖，而 gcc a.c b.c -o test 则是命令。将这行命令用 Makefile 的方式来写就是：

```makefile
test: a.c b.c
    gcc a.c b.c -o test
```

## Go运行命令

Go中支持内置的go命令，可以用来执行：测试、编译、运行、语法检查等命令
一个完善的Go项目经常会执行哪些命令？

+ go vet 静态检查
+ go test 单元测试
+ go fmt 格式化
+ go build 编译
+ go run 运行
+ ...

所以一个适用于Go项目的Makefile也应该支持这些命令

+ make default：编译
+ make fmt：格式化
+ make vet：静态检查
+ make test：运行测试
+ make install：下载依赖库
+ make clean：移除编译的二进制文件

## 引用

[适用于 Go 项目的 Makefile 指南](https://juejin.im/post/5c98edb56fb9a070d75585e3)
[Make 命令教程](http://www.ruanyifeng.com/blog/2015/02/make.html)
[Golang Gin实践 番外 请入门 Makefile](https://github.com/EDDYCJY/blog/blob/master/golang/gin/2018-08-26-Gin%E5%AE%9E%E8%B7%B5-%E7%95%AA%E5%A4%96-%E8%AF%B7%E5%85%A5%E9%97%A8%20Makefile.md)
