---
cover: https://w.wallhaven.cc/full/yx/wallhaven-yx9pzg.jpg
coverY: 250.62607944732298
---

# 🚲 杂七杂八的入门

## printf() & scanf()

* `printf("format string", argument_list)` : 输出函数
* `scanf("format string", argument_list)`  :  输入函数
* `%d` : 数字 、`%f` : 浮点数、 `%s` :字符串 、`%c` : 字符

```c
#include "stdio.h"

int main(){
    int number = 0;
    printf("请输入你的操作数:");
    scanf("%d", &number);
    printf("你输入的操作数为:  %d", number);
}
```

