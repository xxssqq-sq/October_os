### Header（头文件）布局

<img src="img/image-20201223194326814.png" alt="image-20201223194326814" style="zoom:67%;" />

### 防卫式声明 

``` cpp
#ifndef __COMPLEX__
#define __COMPLEX__
....
#endif
```

### 内联函数

inline

### 访问级别access level

public

Private 

### 构造函数 constructor

<img src="/Users/huahua/Documents/%E7%AC%94%E8%AE%B0/img/image-20201221190332265.png" alt="image-20201221190332265" style="zoom:67%;" />

创建对象时，构造函数将被调用

函数名称与类的名称相同，可以拥有参数，参数可以有默认值，不需要返回值。

### Initialization list初值列

```cpp
complex (double r=0,double i=0)
  :re(r),im(i)
  {}
```

变量的赋值有两阶段：初始化、赋值。

在初始化中给值，提高效率。

析构函数

不带指针的不需要写析构函数

### 构造函数可以有很多个-overloading重载

![image-20201221195529284](/Users/huahua/Documents/%E7%AC%94%E8%AE%B0/img/image-20201221195529284.png)

  

### 单例模式singleton

![image-20201223165407860](img/image-20201223165407860.png)

只能有一份实例

创建实例时

`A::getInstance().setup();`

### 常量成员函数

![image-20201223170743862](img/image-20201223170743862.png)const标注表示数据不能被修改  



### pass by value

直接传值

pass by reference(to const)

传首地址，传引用 

引用在底层相当于一个指针，所以传引用相当于传一个指针，但是形式又简洁漂亮。

![image-20201223180842321](img/image-20201223180842321.png)



### 引用

![image-20201223181813510](img/image-20201223181813510.png)

![image-20201223182048498](img/image-20201223182048498.png)

![image-20201223183409160](img/image-20201223183409160.png)

![image-20201223183447793](img/image-20201223183447793.png)

![image-20201223183505678](img/image-20201223183505678.png)

 

### 友元 friends

相同class的各个object互为friends

![image-20201223190233498](img/image-20201223190233498.png)

### 类的编写

数据private

传参reference

看情况做const

构造函数Initialization list

### return by reference  

设有两参数c1、c2

以下有两种情况 1:` c1=c1+c2;`  2:`int b =c1+c2;`

b是在函数中创建的，函数结束该变量即消亡，无法传出引用。

![image-20201223191041082](img/image-20201223191041082.png)

### 操作符重载

操作符是怎样被编译器看待的？

 操作符相当于一种函数 

任何一个成员函数都有一个隐藏的this pointer

### Return by reference 语法分析

<img src="img/image-20201223194110742.png" alt="image-20201223194110742" style="zoom: 50%;" />

