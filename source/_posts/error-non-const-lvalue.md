---
title: error_non_const_lvalue
date: 2021-10-31 00:09:14
tag: C++
---
#### error：cannot bind non-const lvalue reference of type ‘xxx&‘ to an rvalue of type ‘xxx‘
非常量左值引用不能赋给右值


```cpp

class Base{
public:
    Base(){
        cout<<"Base()"<<endl ; 
    }; 
    Base(const Base &other){
        cout<<"Base(Base &other)"<<endl ; 
    }
    virtual void func(){
        cout<<"Base::func()"<<endl; 
    }
};

int main(int argc, char *argv[])
{
    Base b0 ; 
    // 三种调用拷贝构造创建对象的方式
    Base b1(b0) ;  
    Base b2 = b0 ; 
    Base b3 = Base(b0) ;   // 当拷贝构造函数为 Base(Base &other) 而不是Base(const Base &other)时，报错
    return 0;
}
```


**原因：** 
&emsp;&emsp;如果一个参数是以非const引用传入，c++ 编译器就有理由认为程序员会在函数中修改这个值，并且这个被修改的引用在函数返回后要发挥作用。但如果你把一个临时变量当作非const引用参数传进来，由于临时变量的特殊性，程序员并不能操作临时变量，而且临时变量随时可能被释放掉，所以，一般说来，修改一个临时变量是毫无意义的，据此，**c++ 编译器加入了临时变量不能作为非const引用的这个语义限制。**

[参考：https://blog.csdn.net/digitalkee/article/details/105092400](https://blog.csdn.net/digitalkee/article/details/105092400)

