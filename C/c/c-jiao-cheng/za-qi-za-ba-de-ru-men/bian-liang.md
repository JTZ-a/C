---
cover: https://w.wallhaven.cc/full/2y/wallhaven-2y6g36.jpg
coverY: 0
---

# 🚲 变量

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

| Types                 | Data Types                       |
| --------------------- | -------------------------------- |
| Basic Data Type       | int, char, float, double         |
| Derived Data Type     | array, pointer, structure, union |
| Enumeration Data Type | enum                             |
| Void Data Type        | void                             |

## 变量的类型

* 本地变量
* 全局变量
* 静态变量
* 外部变量

{% code title="main.h" %}
```c
extern int extern_var = 0; // 外部变量
```
{% endcode %}

```c
#include "stdio.h"
#include "main.h"

int global_var = 0; // 全局变量
static int static_var = 0; // 静态变量

int main(){
    int local_var = 0; // 本地变量
    printf("Global variable: %d", extern_var);
}
```

## Basic Data Type

<table><thead><tr><th>Data Types</th><th width="153.33333333333331">Memory Size</th><th>Range</th></tr></thead><tbody><tr><td><strong>char</strong></td><td>1 byte</td><td>−128 to 127</td></tr><tr><td><strong>short</strong></td><td>2 byte</td><td>−32,768 to 32,767</td></tr><tr><td><strong>int</strong></td><td>2 byte</td><td>−32,768 to 32,767</td></tr><tr><td><strong>short int</strong></td><td>2 byte</td><td>−32,768 to 32,767</td></tr><tr><td><strong>long int</strong></td><td>4 byte</td><td>-2,147,483,648 to 2,147,483,647</td></tr><tr><td><strong>float</strong></td><td>4 byte</td><td></td></tr><tr><td><strong>double</strong></td><td>8 byte</td><td></td></tr><tr><td><strong>long double</strong></td><td>10 byte</td><td></td></tr></tbody></table>

```c
int main(){
    int a_var = 10; // 有符号数
    unsigned int b_var = 120; // 无符号数
}
```

1. C 语言中默认是有符号数, 如果想要使用无符号数需要使用关键字 `unsigned` &#x20;

## Derived Data Type (派生数据类型)

### 指针

* 介绍 : 用于存储另一种数据类型的内存地址
* 使用事项 :&#x20;
  * `*` :  取出引用地址的值
  * `&` : 取出存储的值

```c
#include "stdio.h"

int main(){
    int* pointer = NULL;
    int var = 10;
    pointer = &var; // 将 var 的变量位置赋值给 pointer
    printf("输出的值: %d", *pointer);
}
```

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>这是最简单的一张原理图</p></figcaption></figure>

### Array

```c
#include <malloc.h>

int main(){
    int definition_array[5];
    int* definition_array2;
    definition_array2 = (int*)malloc(10 * sizeof(int ));
}
```

#### Structure

```c
#include "stdio.h"
#include "string.h"

struct Person{
    char name[50];
    int age;
    float height;
};


int main(){
    struct Person person;
    strcpy(person.name, "JTZ");
    person.age = 30;
    person.height = 1.8f;

    printf("Name: %s \t Age: %d \t Height: %.2f \t", person.name, person.age, person.height);

    return 0;
}
```

### Union

1. 描述 : 各种数据类型存储在同一内存地址中, 与结构体相反, 结构体中每个成员都有自己的内存空间,而 Union 的成员共享一个内容空间, 因此在给定时刻, 一个值只能由联合体中一名成员持有
2. 作用 : 当我们需要灵活的表示多种类型时会使用到此类型
3. 特点 : 占有的内存空间按最大的计算

```c
#include "stdio.h"

union NumericValue{
    char stringValue[20];
    float floatValue;
    int intValue;
};


int main(){
    union NumericValue value;
    value.intValue = 42;
    printf("Integer Value: %d\n", value.intValue);
    value.floatValue = 3.14f;
    printf("Float Value: %.2f\n", value.floatValue);

    return 0;
}
```

## Enumeration Data Type

```c
#include "stdio.h"

enum DaysOfWeek {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
};

int main(){
   enum DaysOfWeek today;
   today = Wednesday;
    printf("Today is %d\n", today);

    return 0;
}
```

## Void Data Type

1. 作为函数返回值
2. 作为函数参数 : 表示该函数不接受任何参数
3. 通用指针 : 类似于 Java 的 Object&#x20;

```c
#include "stdio.h"

void printHello() {
    printf("Hello, world!\n");
}

void processInput(void) {
    printf("Processing input...\n");
}


int main(){
    printHello();
    processInput();
    
    
    int number = 10;
    void* dataPtr = &number;
    printf("Value of number: %d\n", *(int*)dataPtr);
    
    return 0;
}
```

