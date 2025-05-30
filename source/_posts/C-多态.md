---
title: C++多态
date: 2022-02-04 00:04:01
tags: 多态
categories: c++
---
## 什么是多态

面向对象的三大特性：封装、继承、多态。

* 封装：隐藏对象的属性和实现细节，仅对外公开访问方法，并且控制访问级别在面向对象方法中。简言之，用类实现封装，用封装来实现高内聚，低耦合。
* 继承：从已有的类中派生出新的类，新的类能吸收已有类的属性和行为， 可以实现重用代码和扩展新的能力。
* 多态：指通过基类的指针或者引用， **在运行时动态调用实际绑定对象函数的行为**。多态可以简单地概括为“一个接口，多种方法”，它消除类型之间的耦合关系。

从一定角度来看，封装和继承几乎都是为多态而准备的。甚至可以说，**多态是设计模式的实现基础。**
<!--more-->
## 多态的条件

多态的期望行为是**根据实际的对象类型来判断如何调用重写的虚函数**：

* **即当父类指针或引用指向 父类对象时，就调用父类中定义的虚函数**；
* **即当父类指针或引用指向 子类对象时，就调用子类中定义的虚函数**；

即**同样的调用语句在实际运行时有多种不同的表现形态。**根据多态的这种表现效果，C++中实现多态的条件是：

1. 继承的存在；继承是多态的基础，没有继承就没有多态。
2. 子类重写父类的虚方法；多态下调用子类重写的虚方法。
3. 父类指针或引用变量指向子类对象；子类到父类的类型转换。

## 多态的原理

C++支持两种多态性：

* 编译时多态性（静态多态）；通过重载函数实现，也叫先期联编 early binding
* 运行时多态性（动态多态）；通过虚函数实现，也叫滞后联编 late binding

多态执行的大致过程如下：

1. 在类中，用 virtual 声明一个函数时，就会在这个类中对应产生一张 虚函数表，将虚函数存放到该表中；
2. 用这个类创建对象时，就会产生一个 vptr指针，这个vptr指针会指向对应的虚函数表；
3. 在多态调用时, vptr指针 就会根据这个对象 在对应类的虚函数表中 查找被调用的函数，从而找到函数的入口地址；

    * 如果这个对象是 子类的对象，那么vptr指针就会在 子类的 虚函数表中查找被调用的函数
    * 如果这个对象是 父类的对象，那么vptr指针就会在 父类的 虚函数表中查找被调用的函数


![20220204T005410.067](https://raw.githubusercontent.com/leiz2192/myblogimages/main/20220204T005410.067.png)

![20220204T005855.002](https://raw.githubusercontent.com/leiz2192/myblogimages/main/20220204T005855.002.png)

![20220204T010218.310](https://raw.githubusercontent.com/leiz2192/myblogimages/main/20220204T010218.310.png)

![20220204T010241.334](https://raw.githubusercontent.com/leiz2192/myblogimages/main/20220204T010241.334.png)

由于虚函数的存在，在实例化类对象时，就会产生1个 vptr指针。这样，在普通的类中，类的大小 == 成员变量的大小；在有虚函数的类中，类的大小 == 成员变量的大小 + vptr指针大小。

```C++
#include <iostream>
#include <string>

using namespace std;

class Demo1
{
private:
    int mi; // 4 bytes
    int mj; // 4 bytes
public:
    virtual void print(){}
};

class Demo2
{
private:
    int mi; // 4 bytes
    int mj; // 4 bytes
public:
    void print(){}
};

int main()
{
    cout << "sizeof(Demo1) = " << sizeof(Demo1) << " bytes" << endl; // sizeof(Demo1) = 16 bytes
    cout << "sizeof(Demo2) = " << sizeof(Demo2) << " bytes" << endl; // sizeof(Demo2) = 8 bytes
    return 0;
}
```

关于虚函数，需要注意以下几点：

1. 只有类的成员函数才能声明为虚函数，虚函数仅适用于有继承关系的类对象。普通函数不能声明为虚函数。
2. 静态成员函数不能是虚函数，因为静态成员函数不受限于某个对象。
3. 内联函数（inline）不能是虚函数，因为内联函数不能在运行中动态确定位置。
4. 构造函数不能是虚函数。因为在构造函数执行结束后，虚函数表指针才会被正确的初始化。
5. 析构函数可以是虚函数，而且建议声明为虚函数。因为析构函数是在对象销毁之前被调用，即**在对象销毁前**虚函数表指针是正确指向对应的虚函数表。
6. 构造函数中可以调用虚函数，但是不可能发生多态行为，因为在构造函数执行时，虚函数表指针未被正确初始化。
7. 析构函数中可以调用虚函数，但是不可能发生多态行为，因为**在析构函数执行时**，虚函数表指针已经被销毁。

另外，需要注意的是，如果是父类的对象指向子类，则不会发生多态行为：

```C++
#include <iostream>

class Base {
public:
    virtual int add(int value) {
        std::cout << "Base add value: " << value << "\n";
        return 0;
    }
};

class Driver : public Base {
public:
    int add(int value) override {
        std::cout << "Driver add value: " << value << "\n";
        return 0;
    }
};

int main(int argc, char* argv[]) {
    Base b1 = Driver();
    b1.add(22); // output: Base add value: 22

    Base* b2 = new Driver();
    b2->add(22); // output: Driver add value: 22
    return 0;
}
```

## 重写 vs 重载 vs 隐藏

重写：派生类中存在与基类相同的函数（包括函数名、参数列表和参数个数都相同），包括重写成员函数和重写虚函数；其中重写虚函数才能体现C++的多态性。

重载：在同一个类中，允许有多个同名的函数，而这些函数的参数列表不同，允许参数个数不同，参数类型不同，或者两者都不同。

隐藏：指派生类的函数屏蔽了与其同名的基类函数，隐藏规则如下：

* 如果派生类的函数与基类的函数同名，但是参数不同。此时，不论有无virtual 关键字，基类的函数将被隐藏。
* 如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual 关键字。此时，基类的函数被隐藏。

## 参考

[C++中的多态机制](https://www.cnblogs.com/nbk-zyc/p/12274178.html)

[C++ 多态](https://zhuanlan.zhihu.com/p/37340242)
