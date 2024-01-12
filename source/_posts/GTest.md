---
title: GTest
date: 2023-12-17 07:54:28
categories:
tags:
---

## 基本概念
GoogleTest[^1]

test suite 包含1个或多个 tests. 可以把tests组织成test suites以反映测试代码的结构, 当多个测试需要共享数据或流程时可以放在1个类里面

## Assertions
`EXPECT_*`返回非致命错误,会继续运行,推荐使用
`ASSERT_*`从当前函数马上返回,可能跳过它之后的clean-up code, 造成内存泄漏

可在断言处增加自定义打印信息
```
EXPECT_EQ(x[i], y[i]) << "Vectors x and y differ at index " << i;
``` 

简单例子
```
TEST(TestSuiteName, TestName) {
  ... test body ...
}
```
**TestSuiteName, TestName不要使用下划线**

## ubuntu安装gtest
参考[[Ubuntu]GTest安装和测试](https://blog.csdn.net/Swallow_he/article/details/120065786)
ubuntu的开发环境比较友好,可以直接通过命令行下载对应软件包,针对gtest,可以直接在[packages 官网](https://packages.ubuntu.com/search?suite=default&section=all&arch=any&keywords=gtest&searchon=names)搜索, 然后使用`apt`,进行安装, 找到源文件阅读`README.txt`, 进行编译和安装, `make install`把需要的库文件和头文件放在了系统目录

```
sudo apt-get install libgtest-dev
cd /usr/src/gtest

sudo mkdir build # 如果没有文件夹就创建
cd build
sudo cmake ..  #一定要以sudo的方式运行，否则没有写入权限
sudo make      #这个也一样要以sudo的方式
make install
```

创建`hello.txt`文件

```
#include <gtest/gtest.h>

// The fixture for testing class Foo.
class FooTest : public testing::Test
{
protected:
    // You can remove any or all of the following functions if their bodies would
    // be empty.

    FooTest()
    {
        // You can do set-up work for each test here.
    }

    ~FooTest() override
    {
        // You can do clean-up work that doesn't throw exceptions here.
    }

    // If the constructor and destructor are not enough for setting up
    // and cleaning up each test, you can define the following methods:

    void SetUp() override
    {
        // Code here will be called immediately after the constructor (right
        // before each test).
    }

    void TearDown() override
    {
        // Code here will be called immediately after each test (right
        // before the destructor).
    }

    // Class members declared here can be used by all tests in the test suite
    // for Foo.
};

// Tests that the Foo::Bar() method does Abc.
TEST_F(FooTest, MethodBarDoesAbc)
{
    EXPECT_EQ(1, 0) << "1 not equal 0";
}

int main(int argc, char **argv)
{
    std::cout << "hello gtest" << std::endl;
    testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```
编写`CMakeLists.txt`文件
```
cmake_minimum_required(VERSION 3.10.0)
project(test VERSION 0.1.0 LANGUAGES C CXX)

add_executable(
  hello
  hello.cpp
)

target_link_libraries(
  hello
  gtest
  pthread
)
```
不用CMakelist.txt可以直接终端运行

## VScode 配置Gtest
使用插件C++ TestMate, 并设置UT匹配格式,例如`"testMate.cpp.test.executables": "{build,Build,BUILD,out,Out,OUT}/*{UT,test,Test,TEST}*"`, UI设置中也可以找到, 即可在侧边栏找到,实现单个UT运行, 否则直接进入debug则会从第一个UT运行到断点所在UT





[^1]: https://google.github.io/googletest/