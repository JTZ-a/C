---
description: 作为 C 语言的入门介绍
cover: https://w.wallhaven.cc/full/jx/wallhaven-jxdpzy.png
coverY: 49
---

# 🚲 C 入门

## 介绍

* C 语言在整个计算机界都是必要的, 其可以视为其他编程语言的基础 : JVM 、内核都是使用 C 编写的
* C 语言是一种系统级的编程语言, 我们可以利用其编写底层编程 (eg : 驱动程序和内核),同时又因为作为其他高级语言的基础, 因此我们可以将其称为<mark style="color:red;">**中级语言**</mark>
* C 语言是一种接近于底层的语言同时也是最容易入门的一种语言
* C 语言是一种编译型语言
* C 语言是一种有符号的强类型语言

## Hello World!

```c
#include "stdio.h"

int main(){
    printf("Hello World");
}
```

使用 Linux 直接运行

```shell
computer@JTZ:~/C$ gcc hello.c -o hello
computer@JTZ:~/C$ ./hello
Hello World
```
