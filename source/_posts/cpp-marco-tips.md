---
title: C/C++宏的使用技巧 
date: 2021-11-23 17:22:04
tags: C++
---


### 宏的使用技巧

> 1、在带参宏定义中，形式参数不分配内存单元，因此不必作类型定义
>2、\ 用来换行
>3、 # 把变量变为字符串



#### 1.在switch中使代码更简洁
```cpp
string func(int level){
	switch(level){
#define XX(i,name) \
	case i: \
		return name; \
		
	XX(1,"DEBUG") ; 
	XX(2,"INFO") ;
	XX(3,"WARN") ;
	XX(4,"ERROR") ;
	XX(5,"FATAL") ;
#undef XX
	default:
		return "UNKNOW" ; 
	}
	return "UNKNOW" ; 
}
```

#### 2.#把变量变为字符串

```cpp
int main(){
#define STRING(x) (#x)
	cout<<STRING(1)<<endl;
	int a = 1000 ; 
	cout<<STRING(a)<<endl; 
	cout<<STRING(b)<<endl;
	return 0 ; 
}
```

```
输出结果：
1
a
b
```


