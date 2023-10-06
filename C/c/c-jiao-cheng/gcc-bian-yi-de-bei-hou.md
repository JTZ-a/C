---
description: 这一节我们深入 C 语言编译的几个过程来进行了解
cover: https://w.wallhaven.cc/full/zy/wallhaven-zy9e8g.png
coverY: -167
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🚲 Gcc 编译的背后

## 前言

在以往的学习过程中, 我们知道 C 语言的执行的四大流程 : 预处理、编译、汇编、链接,&#x20;

在这里我偷个懒使用 C++ 来作为表示 (两个语言的处理流程大致都一样)

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>来源: https://hackingcpp.com/cpp/hello_world.html</p></figcaption></figure>

## Preprocessor (预处理)

预处理是 C 程序从源代码变成可执行程序的第一步, 在这一步中编译器主要做的操作是:

1. 预处理指令处理：预处理器会扫描源代码中以 `#` 开头的预处理指令，并根据指令进行相应的处理。常见的预处理指令包括 `#include`、`#define`、`#ifdef`、`#ifndef`、`#if`、`#else`、`#endif` 等。预处理指令可以用于包含头文件、宏定义、条件编译等操作。
2. 头文件包含：通过 `#include` 指令，预处理器可以将其他头文件的内容插入当前源文件中。这样可以在源代码中使用其他文件中定义的函数、变量和类型声明，提供了代码的模块化和重用性。
3. 宏展开：预处理器会处理 `#define` 指令定义的宏，并将宏在代码中的引用展开为宏定义的内容。这样可以在源代码中使用宏来进行代码替换，提高代码的可读性和维护性。
4. 条件编译：通过 `#ifdef`、`#ifndef`、`#if` 等条件编译指令，预处理器可以根据条件判断选择性地编译部分代码。这样可以根据不同的编译选项或环境条件，在不同的情况下编译不同的代码。
5. 注释删除：预处理器会删除源代码中的注释，这样注释的内容在编译过程中将不被考虑。

> 主要还是归属于两步 : 预处理指令的处理以及注释删除

在这里我们可以通过下面的命令来查看预处理的输出 (有点长), 从输出中我们可以看到其主要做的操作, 其实可以将其归属于资源的准备

```sh
$ gcc -E hello.c
```

## 编译

在编译之前, C 语言编译器会进行词法分析、语法分析, 接着会将我们的源代码<mark style="color:red;">**编译**</mark>成中间语言, 即汇编语言

> 1. 如果我们使用编译器比如 VS 、Clion  等编译器的话, 我们在编写其间也会进行语法的分析, 但是这与编译阶段的语法分析没有任何关系
> 2. 对于解释型语言来说, 也会经历词法分析、语法分析阶段, 不管之后<mark style="color:red;">**并不会进行编译**</mark>, 而是 "解释" , 边解释边执行

{% tabs %}
{% tab title="README" %}
通常来说我们会执行下面的语言来实现编译的过程

```bash
$ gcc -S hello.c
```
{% endtab %}

{% tab title="语法检查" %}
如果仅仅希望进行语法检查，可以用 `gcc` 的 `-fsyntax-only` 选项

```sh
┌──(jtz㉿JTZ)-[~/C]
└─$ cat hello.c
#include <stdio.h>
int main(){
    printf("hello world")
    return 0;
}

┌──(jtz㉿JTZ)-[~/C]
└─$ gcc -fsyntax-only hello.c
hello.c: In function ‘main’:
hello.c:3:26: error: expected ‘;’ before ‘return’
    3 |     printf("hello world")
      |                          ^
      |                          ;
    4 |     return 0;
      |     ~~~~~~

```
{% endtab %}

{% tab title="编译" %}
当语法检查通过之后就该执行编译操作, 将源代码编译为汇编语言

```sh
┌──(jtz㉿JTZ)-[~/C]
└─$ gcc -S hello.c

┌──(jtz㉿JTZ)-[~/C]
└─$ cat hello.s
        .file   "hello.c"
        .text
        .section        .rodata
.LC0:
        .string "hello world"
        .text
        .globl  main
        .type   main, @function
main:
.LFB0:
        .cfi_startproc
        pushq   %rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        movq    %rsp, %rbp
        .cfi_def_cfa_register 6
        leaq    .LC0(%rip), %rax
        movq    %rax, %rdi
        movl    $0, %eax
        call    printf@PLT
        movl    $0, %eax
        popq    %rbp
        .cfi_def_cfa 7, 8
        ret
        .cfi_endproc
.LFE0:
        .size   main, .-main
        .ident  "GCC: (Debian 12.2.0-14) 12.2.0"
        .section        .note.GNU-stack,"",@progbits

```
{% endtab %}
{% endtabs %}

## 汇编

使用汇编器将汇编代码转换为目标代码, 在 DOS 中, 目标文件的扩展名为 `.obj` 在 UNIX 中扩展名 `.o`&#x20;

```bash
┌──(jtz㉿JTZ)-[~/C]
└─$ file hello.o
hello.o: cannot open `hello.o' (No such file or directory)
┌──(jtz㉿JTZ)-[~/C]
└─$ gcc -C hello.s -o hello.o

┌──(jtz㉿JTZ)-[~/C]
└─$ file hello.o
hello.o: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=87500146a349df57a9e19eb533796c446865a5b2, for GNU/Linux 3.2.0, not stripped
```

## 链接

> 通常使用 C 编写的程序会使用库函数, 这些库函数已经预先编译好了, 库文件一般使用 `.lib` 或者 `.a` 扩展名存储

1. 在此阶段的主要工作是将库文件的目标代码和我们程序的目标代码组合起来, 比如当我们在程序中调用在其他文件中调用的函数时, 链接器会将这些文件的目标代码与我们的程序链接起来. 从此我们可以得出结论 : <mark style="color:red;">**链接器的任务是将我们程序的目标代码与库文件和其他文件的目标代码链接起来**</mark>.&#x20;
2. 链接器的输出是可执行文件, 生成的可执行文件的扩展名在 DOS 中为 `.exe` 在 UNXI 中为 `.out`
3. 链接器出来讲目标代码与库文件的目标代码进行链接生成可执行文件外, 还会负责解析符号引用, 以确保所有函数和变量都能正确链接在一起

