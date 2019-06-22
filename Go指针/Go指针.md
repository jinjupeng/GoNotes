# Golang指针

[](https://www.runoob.com/go/go-pointers.html)
[理解Go指针](https://studygolang.com/articles/20139)

## 指针概念

+ 变量存储的是一个值，但是这个值在内存中有一个地址，而指针保存的就是这个地址，通过这个地址，可以获取到值。
+ 指针就是一个指向另一个内存地址变量的值。
+ 指针指向变量的内存地址，指针就像该变量值的内存地址一样。

![Go指针](https://github.com/jinjupeng/GoNotes/blob/master/Go%E6%8C%87%E9%92%88/Img/Go%E6%8C%87%E9%92%88%E5%9B%BE%E7%A4%BA.jpeg)

```go
package main
import "fmt"

func main() {
    var a = 10
    var p *int
    fmt.Println("声明int类型指针p = ", p)
    p = &a
    fmt.Println("a的内存地址 = ", &a)
    fmt.Println("a内存地址赋值给p，指针p保存的值 = ", p)
    fmt.Println("*号将p内保存的地址所指向的值取出 *p = ", *p)
}

# 输出
声明int类型指针p =  <nil>
a的内存地址 =  0xc00006a080
a内存地址赋值给p，指针p保存的值 =  0xc00006a080
*号将p内保存的地址所指向的值取出 *p =  10
```

## Go指针数组

```go
package main

import "fmt"

const MAX int = 3

func main() {
    a := []int{10,100,200}
    var i int
    var ptr [MAX]*int;
    for  i = 0; i < MAX; i++ {
        ptr[i] = &a[i] /* 整数地址赋值给指针数组 */
    }
    for  i = 0; i < MAX; i++ {
        fmt.Printf("a[%d] = %d\n", i,*ptr[i] )
    }
}
# 输出
a[0] = 10
a[1] = 100
a[2] = 200
```

## Go指向指针的指针

> 如果一个指针变量存放的是另一个指针变量的地址，则称这个指针变量为指向指针的指针变量。
> 当定义一个指向指针的指针变量时，第一个指针存放的是第二个指针的地址，而第二个指针是存放变量的地址。

![指针的指针](https://github.com/jinjupeng/GoNotes/blob/master/Go%E6%8C%87%E9%92%88/Img/%E6%8C%87%E9%92%88%E7%9A%84%E6%8C%87%E9%92%88.jpg)

```go
package main

import "fmt"

func main() {

    var a int
    var ptr *int
    var pptr **int

    a = 3000

    // 指针 ptr 地址
    ptr = &a

    // 指向指针 ptr 地址
    pptr = &ptr

    // 获取 pptr 的值 
    fmt.Printf("变量 a = %d\n", a )
    fmt.Printf("变量 a的地址 = %d\n", &a )
    fmt.Printf("指针ptr保存的值就是变量a的地址 = %d\n", ptr )
    fmt.Printf("指针ptr的地址 = %d\n", &ptr )
    fmt.Printf("指针变量 *ptr获取a的值 = %d\n", *ptr )
    fmt.Printf("指针的指针pptr保存的值就是指针ptr的地址 = %d\n", &ptr )
    fmt.Printf("指向指针的指针变量 **pptr = %d\n", **pptr)
}
# 输出
变量 a = 3000
变量 a的地址 = 824634155136
指针ptr保存的值就是变量a的地址 = 824634155136
指针ptr的地址 = 824634335256
指针变量 *ptr获取a的值 = 3000
指针的指针pptr保存的值就是指针ptr的地址 = 824634335256
指向指针的指针变量 **pptr = 3000
```

## 向函数传递指针参数

> Go语言允许向函数传递指针，只需要在函数定义的参数上设置为指针类型即可。
> 通过引用或地址传参，在函数调用时可以改变其值

```go
package main

import "fmt"

func main() {
    // 定义局部变量
    var a = 100
    var b = 200
    var c = 199
    var d = 299

    fmt.Printf("交换前 a 的值 : %d\n", a )
    fmt.Printf("交换前 b 的值 : %d\n", b )
    fmt.Printf("交换前 c 的值 : %d\n", c )
    fmt.Printf("交换前 d 的值 : %d\n", d )
    fmt.Println("------分割线------" )

    /* 调用函数用于交换值
     * &a 指向 a 变量的地址
     * &b 指向 b 变量的地址
     */
    swap(&a, &b)
    /* 值传递
     * 没有修改原始值
     * 只修改副本值
     */
    swap2(c, d)

    fmt.Printf("交换后 a 的值 : %d\n", a )
    fmt.Printf("交换后 b 的值 : %d\n", b )
    fmt.Printf("交换后 c 的值 : %d\n", c )
    fmt.Printf("交换后 d 的值 : %d\n", d )
}

// 引用传递
func swap(x *int, y *int) {
    var temp int
    temp = *x    /* 保存 x 地址的值 */
    *x = *y      /* 将 y 赋值给 x */
    *y = temp    /* 将 temp 赋值给 y */
    // *x, *y = *y, *x 也支持
}
// 值传递，重新创建一份副本，只修改了副本，原始数据并未修改
func swap2(x int, y int) {
    var temp int
    temp = x
    x = y
    y = temp
}
# 输出
交换前 a 的值 : 100
交换前 b 的值 : 200
交换前 c 的值 : 199
交换前 d 的值 : 299
------分割线------
交换后 a 的值 : 200
交换后 b 的值 : 100
交换后 c 的值 : 199
交换后 d 的值 : 299
```

## 值传递和引用传递

![区别](https://github.com/jinjupeng/GoNotes/blob/master/Go%E6%8C%87%E9%92%88/Img/%E5%80%BC%E4%BC%A0%E9%80%92%E5%92%8C%E5%BC%95%E7%94%A8%E4%BC%A0%E9%80%92.gif)
