---
title: C++可变参数 
date: 2021-11-24 22:19:19
tags: C++
---

### 参数列表的...
... 表示函数的参数个数可变，典型的如`printf()`
```cpp
int printf (const char * szFormat, ...);
```
第一个参数是一个格式化字符串，后面是与格式化字符串中的代码相对应的不同类型的多个参数。

```cpp
char* name = "fgq";
int age = 18; 
printf("info {name:%s, age:%d}\n",name, age) ; 
```

### 使用...实现变参数函数的两种场景
#### 1. 格式化字符串

> **使用场景：**
> 类似于实现一个printf，输入一串格式化的字符串，经过处理后可以将 %s %f %d等占位符替换为对应的数据。


```cpp
char* func(const char* format, va_list ap)  
{
	char* buf = nullptr;
    auto len = vasprintf(&buf, format, ap); 
    if(len == -1) {
        return buf;
    }
    return buf; 
}

char* func(const char* format, ...)  // 重载了func函数，不重载也行
{
    va_list ap;
    char *res = NULL; 
    va_start(ap, format);
    res = func(format, ap);  
    va_end(ap);
    return res ;  
}   

int main(){
    char* name = "fgq";
    int age = 18; 
    char* str = func("info {name:%s, age:%d}\n",name, age) ;
    printf("result: %s \n", str) ; 
    return 0;  
}
```

```
输出结果
result: info {name:fgq, age:18}
```

> **注意：**
> 上面func()函数重载了，如果不是类成员函数，要注意函数定义的顺序，在func(const char* format, ...)里要调用`func(const char* format, va_list ap)` ，因此`func(const char* format, ...)`要定义在后面。类内成员函数则可以是任意顺序。

实际上完成占位符替换为数据的是`int vasprintf (char **buf, const char *format, va_list ap)`函数：
`buf`：一个用于保存结果的字符串缓冲区
`format`：一个格式化字符串
`ap`:va_list类型的变量, va_list是一个宏，和va_start(va_list, arg)、va_arg(va_list, type)、va_end(va_list)这些宏在定义在头文件stdarg.h中，下面详细介绍通过这些宏来实现可变参数函数

#### 2.执行时指定可变参数类型

> **使用场景：**
> 将n个数进行相加，此时n是不确定的，如果用重载方法，可能要重载很多次，如下面的例子

```cpp
例子：
int sum(int i1, int i2);
int sum(int i1, int i2, int i3);
...//还需要重载更多类似函数
 
double sum(double d1, double d2);
double sum(double d1, double d2, double d3);
...//还需要重载更多类似函数
```
使用可变参数的方法：
```cpp

int sum(int count, ...) {  //格式:count代表参数个数, ...代表n个参数
 
	va_list ap;  //声明一个va_list变量
	va_start(ap, count);  //第二个参数表示形参的个数
 
	int res = 0;
	for (int i = 0; i < count; i++) { // 按顺序返回参数列表中的参数
		res += va_arg(ap, int);   //第二个参数表示形参类型
	}
	
	va_end(ap);  //用于清理
 
	return res;
}

int main(){
    int res = sum(3,1,2,3);
    cout<<res<<endl; 
    return 0; 
}
```

```
输出结果：
6
```
### 使用initializer_list实现变参函数
`initializer_list`是一个列表初始化容器，声明在initializer_list头文件中，可以采用迭代器的方式来遍历参数列表，克服了`...`需要指定参数个数的缺点。

```cpp
int initializerSum(initializer_list<int> il) {
   
	int sum = 0;
 
	for (auto ptr = il.begin(); ptr != il.end(); ptr++)  //类似于容器的操作
	{
		sum += *ptr;
	}
 
	return sum;
}

int main(){
    int res = initializerSum({1,2,3}) ;  // 初始化 initializer_list<int>
    cout<<res<<endl; 

    std::initializer_list<int> il {1} ;
    res = initializerSum(il);
    cout<<res<<endl; 

    std::initializer_list<int> ill ({1,2}) ;
    res = initializerSum(ill);
    cout<<res<<endl; 
    
    auto illl = {1,2,3,4};
    res = initializerSum(illl);
    cout<<res<<endl;
    
    return 0 ; 
}
```

参考：
[cppreference对inittializer_list的介绍](http://en.cppreference.com/w/cpp/utility/initializer_list)

[va_list函数学习（va_start，va_end, vasprintf）](https://blog.csdn.net/baidu_15952103/article/details/105886761)

[C++可变参数的两种方法](https://blog.csdn.net/alex1997222/article/details/78639991)

[printf,sprintf,vsprintf 区别](https://blog.csdn.net/anye3000/article/details/6593551)
