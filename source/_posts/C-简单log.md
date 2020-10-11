---
title: C++简单log
date: 2020-10-11 15:37:42
tags: loging
categories: c++
---

平常写一些简单验证程序时，需要频繁输出日志信息便于调试。为了后续验证程序中输出日志信息方便，将平时使用的日志信息抽象成了INFO和FUNC。

- INFO输出单行日志信息，可以输出当前的时间（格式：%Y-%m-%d %H:%M:%S）、进程号和线程号。
- FUNC利用RAII可以在生命周期（比如函数）的开始和结束分别打印日志信息，并统计执行耗时（单位微妙）
<!--more-->

```c++
#include <iostream>
#include <unistd.h>
#include <chrono>
#include <iomanip>
#include <ctime>
#include <sstream>
#include <string>

#define INFO(msg) \
do { \
    auto now = std::chrono::system_clock::now(); \
    auto t = std::chrono::system_clock::to_time_t(now); \
    auto fmt_t = std::put_time(std::localtime(&t), "%Y-%m-%d %H:%M:%S"); \
    std::ostringstream oss; \
    oss << fmt_t << " [" << getpid() << "|" << gettid() << "] " << msg << "\n"; \
    std::cout << oss.str(); \
} while (0)

class _UniInfo {
public:
    _UniInfo(const std::string& msg) : msg_(msg) {
        start_ = std::chrono::system_clock::now();
        INFO(msg_ << " Enter");
    }

    ~_UniInfo() {
        INFO(msg_ << " Exit");
        auto end = std::chrono::system_clock::now();
        auto elapsed = std::chrono::duration_cast<std::chrono::microseconds>(end - start_).count();
        INFO(msg_ << " elapsed time: " << elapsed << " microseconds");
    }
private:
    std::string msg_;
    std::chrono::system_clock::time_point start_;
};

#define FUNC(msg) \
    std::ostringstream _oss; \
    _oss << msg; \
    _UniInfo _uniInfo(_oss.str());


#define FUNCTION() FUNC(__func__)

int main(int argc, char* argv[]) {
    FUNCTION();
    return 0;
}
```
