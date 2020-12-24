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

![image-20201221190332265](/Users/huahua/Documents/%E7%AC%94%E8%AE%B0/img/image-20201221190332265.png)

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

![image-20201223170743862](img/image-20201223170743862.png)

