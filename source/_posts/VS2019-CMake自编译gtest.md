---
title: VS2019 CMake编译gtest
date: 2020-06-14 01:02:16
tags: c++
categories: c++
---

- 从[GitHub](https://github.com/google/googletest)下载gtest源码。

- 从VS2019选择和创建CMake项目。
<!--more-->
![select](/images/20200614T215958.440.png)
![create](/images/20200614T220154.311.png)

- 将下载的gtest源码解压到项目目录下。
![srczip](/images/20200614T222711.295.png)

- 修改项目的CMakeLists.txt文件如下。

    ```c++
    cmake_minimum_required (VERSION 3.8)

    project ("HelloCMakeGtest")

    set(GTEST googletest-1.10.x)

    add_subdirectory(${GTEST})
    include_directories(${GTEST}/googletest/include/)

    add_executable (HelloCMakeGtest "HelloCMakeGtest.cpp" "HelloCMakeGtest.h")

    target_link_libraries(LintCodeCMakeProject gtest)
    ```

- 在项目的CMake设置中将“gtest_force_shared_crt”设置为True。注意需要“显示高级设置”和“显示高级变量”。不然编译会存在问题。原因是gtest默认是静态编译，与VS项目设置不一致。
![shared](/images/20200614T224131.819.png)

- 修改项目代码文件。

    ```c++
    #include "HelloCMakeGtest.h"
    #include "gtest/gtest.h"

    int add(int n1, int n2) {
        return n1 + n2;
    }

    TEST(TestCase, test1) {
        ASSERT_EQ(12, add(4, 8));
    }

    TEST(TestCase, test2) {
        EXPECT_EQ(5, add(2, 3));
    }

    TEST(TestCase, test3) {
        EXPECT_EQ(3, add(1, 2));
    }

    int main(int argc, char* argv[]) {
        ::testing::InitGoogleTest(&argc, argv);
        return RUN_ALL_TESTS();
    }
    ```

- 全部生成并开始执行。后续添加需要测试用例和修改CMakeLists.txt即可。
![makerun](/images/20200614T225131.721.png)
