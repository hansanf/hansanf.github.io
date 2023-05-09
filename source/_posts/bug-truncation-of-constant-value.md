---
title: bug记录：常量值截断(truncation of constant value) 
date: 2022-05-05 23:38:42
tags: C++
categories: Bug 
---

### char和unsigned char
**char的取值范围**：-128~127
&ensp;&ensp;-128: 16进制0x80  二进制1000 0000 
&ensp;&ensp;-127: 16进制0xff   &ensp;二进制1111 1111，注意第一位是符号位，0111 1111是127

**unsigned char的取值范围**：0~255，16进制0x00 ~ 0xff


### 内存中的0xf0: 1111 0000
```cpp
char x = 0xf0; // 十进制数大小为240，超出了127，导致常量值截断
if(0xf0 == x){ // x86机器上判断为false，arm机器上可能判断为true
	std::cout << "x == 0xf0" << std::endl;
}
std::cout<< "char x= " << x 
		<< "\tunsigned char x = " << (unsigned char)x 
		<< "\tint x = " << (int)x << std::endl; // 在不同机器上可能出现不同值
```
mac(m1 arm64)打印结果：

```
	char x= �	unsigned char x = �	int x = -16  
```
linux(x86)打印结果和上面一致

arm盒子(arm64)打印结果：
```
	char x= �	unsigned char x = �	int x = 240
```
&ensp;&ensp;&ensp;&ensp; char和unsigned char乱码，直接看int，一个取值-16，一个取值240

**取值为-16的情况：**

&ensp;&ensp;&ensp;&ensp; int类型时，-16的二进制为 1000 0000| 0000 0000| 0000 0000| 0001 0000，最后一个字节是0001 0000

&ensp;&ensp;&ensp;&ensp; 而1111 0000 取反为 0000 11111，加1正好是0001 0000，即0xf0的补码是0001 0000，因此在将(char)0xf0变为int类型时的十进制数-16，至于为什么是这样的规则我也不知道。
**取值为240的情况：**

&ensp;&ensp;&ensp;&ensp; int类型时，240的二进制为 0000 0000| 0000 0000| 0000 0000| 1111 0000，最后一个字节就是1111 0000

&ensp;&ensp;&ensp;&ensp;综上，有些机器在处理常量截断时是取补码，而有些机器是取正码，即不变。实际上，`char x = 0xf0`这种用法本身就是错误的，不应该使用，对二进制字节比较时最好选用`unsigned char`类型。

&ensp;&ensp;&ensp;&ensp;不过，在对内存中的二进制字节进行判断时，也可以使用 if(x == (char)0xf0)这种形式。

**Note & 猜测:**

&ensp;&ensp;&ensp;&ensp; `char x = 0xf0` 虽然发生了常量截断，但是内存中的值确实是0xf0，只不过在从内存中读取出来时不同类型有不同的取值。

参考：

[http://www.cplusplus.com/forum/beginner/48577/](http://www.cplusplus.com/forum/beginner/48577/)
[https://zhidao.baidu.com/question/533897399.html](https://zhidao.baidu.com/question/533897399.html)
