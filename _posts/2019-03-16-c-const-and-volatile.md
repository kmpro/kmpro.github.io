---
layout:     post
title:      "C程序中的const和volatile的作用"
date:       2018-12-9 15:15:00
author:     "KM"
catalog: true
tags:
    - Java

---

# C程序中的const和volatile的作用

## const

### 1.被const修饰的变量值不能被改变吗？

如果一个变量被const修饰，那么意味着无法通过它的变量名去修改它的值，这就是一个常量。

如：

```c
#include <stdio.h>

int main(void)
{
    const int a = 1;
	a = 2;
    printf("a:%d\r\n", a);
    return 0;
}
```

很明显在编译上面的程序编译器是会报错的，提示a是一个只读变量，不允许修改。

但是这样并不意味着它的值不可以通过其他方式被修改，如可以通过指针的方式直接修改内存的内容

```c
#include <stdio.h>

int main(void)
{
    volitile const int a = 1;
	int *b = &a;
	*b = 2;
    printf("a:%d\r\n", a);
    return 0;
}
```

执行之后发现a的值变成了2，说明const只能保证变量不被修改（即通过变量名赋值的方式），但不能保证内存区域的内容不能被修改。

### 2.const修饰指针

const int * pOne;    //指向**整形常量** 的指针，它指向的值不能修改

int * const pTwo;    //指向整形的**常量指针** ，它不能在指向别的变量，但指向（变量）的值可以修改。 

const int *const pThree;  //指向**整形常量** 的**常量指针** 。它既不能再指向别的常量，指向的值也不能修改。

## volatile

被volatile修饰的变量意味着该变量可能会出现意外的变化，需要程序每次都去内存加载变量的值，而不是直接使用寄存器的值，因为有可能内存的值已经更新。

```c
#include <stdio.h>

int main(void)
{
    const char i = 1;
    char *j = (char *)&i;
    *j = 2; 
    printf("%d,%d\n",i,*j);//1,2
}
```

上面i的值已经被修改为2，但是编译器认为i是个const类型，它的值应该不会出现变化，则在编译printf时默认将i的默认值1赋给了它。加上volatile就可以强制加载i的值，就不会出现问题了。