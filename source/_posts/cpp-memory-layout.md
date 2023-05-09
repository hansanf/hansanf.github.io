---
title: C++类和对象的内存布局
date: 2021-11-01 07:08:12
tags: C++
---
 
### 用g++查看内存布局的方法：
&emsp;&emsp;g++ 版本>8.0：`g++ -fdump-lang-class vptr.cpp`
&emsp;&emsp;g++ 版本<8.0：`g++ -fdump-class-hierarchy vptr.cpp`
[参考：https://blog.csdn.net/Ineedapassward/article/details/118417116](https://blog.csdn.net/Ineedapassward/article/details/118417116)

### 类的内存布局
[参考：https://blog.csdn.net/shichao1470/article/details/91563282](https://blog.csdn.net/shichao1470/article/details/91563282)

### 菱形继承下对象的内存布局
[参考：https://blog.csdn.net/j4ya_/article/details/80177897](https://blog.csdn.net/j4ya_/article/details/80177897)

### 菱形继承下类的内存布局

```cpp

class Base{
public:
    virtual void func(){
        cout<<"Base::func()"<<endl; 
    }
};

class X: virtual public Base{
    void func(){
        cout<<"X::func()"<<endl; 
    }
};

class Y: virtual public Base{
    void func(){
        cout<<"Y::func()"<<endl; 
    }
    virtual void funcY(){}
};
class Z: public X,  public Y{
    void func(){
        cout<<"Z::func()"<<endl; 
    }
};



int main(int argc, char *argv[])
{
    cout<<sizeof(Base)<<endl
        <<sizeof(X)<<endl
        <<sizeof(Y)<<endl
        <<sizeof(Z)<<endl ; 
    
    return 0;
}
```
64位系统下运行，sizeof(int *) 等于8，输出结果为：

```bash
8
8
8
16
```

**问题：**
&emsp;&emsp;类Base的size为8，是因为有一个虚表指针，
&emsp;&emsp;类X和类Y的size也为8，也是因为各自只有一个虚表指针？
&emsp;&emsp;类Z的size为16，是为甚？不采用虚继承的时候结果不变，为甚？
使用g++ 查看内存布局，结果如下：

```
Vtable for Z
Z::_ZTV1Z: 6 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1Z)
16    (int (*)(...))Z::func
24    (int (*)(...))-8
32    (int (*)(...))(& _ZTI1Z)
40    (int (*)(...))Z::_ZThn8_N1Z4funcEv

Class Z
   size=16 align=8
   base size=16 base align=8
Z (0x0x3feb780) 0
    vptr=((& Z::_ZTV1Z) + 16)
  X (0x0x3feb7c0) 0 nearly-empty
      primary-for Z (0x0x3feb780)
    Base (0x0x3fdca48) 0 nearly-empty
        primary-for X (0x0x3feb7c0)
  Y (0x0x3feb800) 8 nearly-empty
      vptr=((& Z::_ZTV1Z) + 40)
    Base (0x0x3fdca80) 8 nearly-empty
        primary-for Y (0x0x3feb800)
```
为啥

```
  Y (0x0x3feb800) 8 nearly-empty
      vptr=((& Z::_ZTV1Z) + 40)
    Base (0x0x3fdca80) 8 nearly-empty
        primary-for Y (0x0x3feb800)
```
这里是什么东西啊？


