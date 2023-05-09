---
title: 设计模式之类图表示
date: 2021-11-07 21:13:22
tags: 设计模式
categories: 设计模式
---
### 类图属性
一个大矩形里面分三层：
- 类名
- 成员名：可见性 名称 ：类型 [ = 默认值]
- 方法名：可见性  名称(参数列表) [ ： 返回类型]
![在这里插入图片描述](https://img-blog.csdnimg.cn/c59f02761f6b4d5c926297de49d48cfa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_20,color_FFFFFF,t_70,g_se,x_16)

可见性：
- +：表示public
- -：表示private
- #：表示protected（friendly也归入这类）

### 类之间关系
#### 1、依赖
依赖关系使用**带箭头的虚线**来表示，箭头从使用类指向被依赖的类。

**人依赖手机：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f53bd1f094a946b9a8121fe7f7c3beb9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_20,color_FFFFFF,t_70,g_se,x_16)
依赖（Dependency）关系是一种**使用关系**，它是对象之间耦合度最弱的一种关联方式，被使用对象的有无，不影响使用对象，是临时性的关联。

#### 2、关联
依赖关系使用**带箭头的实线**来表示，箭头从使用类指向被关联的类，可以单向关联，也可以双向关联。

双向的关联可以用带两个箭头或者没有箭头的实线来表示，单向的关联用带一个箭头的实线来表示。

**老师和学生双向关联：**每个老师可以教多个学生，每个学生也可向多个老师学
![在这里插入图片描述](https://img-blog.csdnimg.cn/59ed529d91fb40fcb133824a7fc0f59b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_20,color_FFFFFF,t_70,g_se,x_16)
关联（Association）关系是对象之间的一种**引用关系**，用于表示一类对象与另一类对象之间的联系，如老师和学生、师傅和徒弟、丈夫和妻子等。被引用者不存在时，引用者可以存在，但是没有意义。

#### 3、聚合
聚合关系可以用**带空心菱形的实线**来表示，菱形指向整体。

**大学包含教师：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/5979cb34f002423bb819274d3a3798d0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_20,color_FFFFFF,t_70,g_se,x_16)


聚合（Aggregation）关系是关联关系的一种，是强关联关系，是**整体和部分之间的关系**，是 has-a 的关系。当部分类不存在，整体类也不能存在，但是部分可以脱离整体而独立存在。比如大学不能脱离教师而存在，但是没有大学教师依然能够独立存在。

#### 4、组合
组合关系用**带实心菱形的实线来表示**，菱形指向整体
**头和嘴：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/9accac46ce1d4eb382cdf8d1c00623b6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_20,color_FFFFFF,t_70,g_se,x_16)

在组合关系中，整体对象可以控制部分对象的生命周期，一旦整体对象不存在，部分对象也将不存在，部分对象也不能脱离整体对象而存在。例如，头和嘴的关系，没有了头，嘴也就不存在了。

#### 5、泛化
泛化关系用**带空心三角箭头的实线**来表示，箭头从子类指向父类。

**人和学生、老师：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/b27b8f34dcfd4134b179533991d8d045.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_20,color_FFFFFF,t_70,g_se,x_16)

泛化（Generalization）关系是对象之间耦合度最大的一种关系，表示一般与特殊的关系，是**父类与子类之间的关系，是一种继承关系**，是 is-a 的关系。

#### 6、实现
实现关系使用**带空心三角箭头的虚线来表示**，箭头从实现类指向接口

**鸟和飞：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2380c43ba2dc4c07b395882e3930059c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_15,color_FFFFFF,t_70,g_se,x_16)

实现（Realization）关系是**接口与实现类之间的关系**。在这种关系中，类实现了接口，类中的操作实现了接口中所声明的所有的抽象操作。可以将实现类看做方法。

参考：
[https://www.cnblogs.com/shindo/p/5579191.html](https://www.cnblogs.com/shindo/p/5579191.html)
[https://blog.csdn.net/sinat_21107433/article/details/102576624](https://blog.csdn.net/sinat_21107433/article/details/102576624)
[http://c.biancheng.net/view/1319.html](http://c.biancheng.net/view/1319.html)
[https://www.jianshu.com/p/641682f9c918](https://www.jianshu.com/p/641682f9c918)
