---
title: C++ Pimpl技术
date: 2022-01-16 19:04:52
tags: pimpl
categories: c++
---
## 什么是Pimpl技术

Pimpl，Pointer to Implementation，意为“具体实现的指针”，有着“编译防火墙(compilation firewall)”的名头。

它通过一个私有的成员指针，将指针所指向的类的内部实现数据进行隐藏，降低耦合性和分离接口实现的一个现代 C++ 技术。
<!--more-->
## Pimpl代码示例

View类定义：

```cpp
#pragma once
#include <memory>

class View {
public:
    View();
    ~View();
    View(View&& v);
    View& operator=(View&& v);

    void display();

private:
    class Impl;
    std::unique_ptr<Impl> pimpl;
};
```

View类实现：

```cpp
#include <iostream>
#include <string>

#include "view.h"

//View::Impl的定义，体现了View类的具体实现细节
class View::Impl {
    std::string name;
public:
    Impl();
    void printName();
};

View::Impl::Impl(){
    name = "ViewImpl";
}

void View::Impl::printName(){
    std::cout << "this is my name:" << name << "\n";
}

//View类接口的实现
View::View():pimpl(std::make_unique<Impl>()){
}

View::~View() = default;
View::View(View&& v) = default;
View& View::operator = (View&& v) = default;

void View::display(){
    pimpl->printName();
}
```

View使用：

```cpp
#include <iostream>

#include "view.h"

int main(int argc, char* argv[]) {
    std::cout << "Hello C++ World\n";

    View v = View();
    v.display();
}
```

编译：

```bash
$ g++ main.cpp view.cpp
```

输出：

```bash
$ ./a.out
Hello C++ World
this is my name:ViewImpl
```

## Pimpl技术优缺点

优点：

* 最大程度地减少编译依赖项：其一减少原类不必要的头文件的依赖，加速编译；其二对Impl类进行修改，无需重新编译原类；
* 接口和实现分离，降低模块的耦合：私有成员隐藏在公有接口之外，给用户一个简洁明了的使用接口， 适合闭源API设计；

缺点：

* 需要占用额外的指针内存；
* 访问时多一次间接指针操作的开销；
* 在使用、阅读和调试上都可能有所不便；

## 参考

[C++ 编译期封装-Pimpl技术](https://www.cnblogs.com/KillerAery/p/9539705.html)

[用于编译时封装的 Pimpl（现代 C++）](https://docs.microsoft.com/zh-cn/cpp/cpp/pimpl-for-compile-time-encapsulation-modern-cpp?view=msvc-170)