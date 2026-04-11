---
title: c++记录
date: 2019-10-06 16:35:06
categories:
- Program

tags:
- c++
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

- [g++编译](#g编译)
  - [常用选项](#常用选项)
- [enum 枚举类型](#enum-枚举类型)
- [左值 右值](#左值-右值)
- [变量初始化](#变量初始化)
- [限定符](#限定符)
- [存储类](#存储类)
- [运算符优先级](#运算符优先级)
- [lambda 函数与表达式](#lambda-函数与表达式)
  - [qt中的lambda表达式](#qt中的lambda表达式)
- [指向指针的指针](#指向指针的指针)
- [虚继承](#虚继承)
- [运算符重载](#运算符重载)
- [虚函数](#虚函数)
- [c++ STL](#c-stl)
- [文件和流](#文件和流)
  - [文件输入输出流](#文件输入输出流)
  - [字符串输入输出流](#字符串输入输出流)
- [异常处理](#异常处理)
  - [异常的使用](#异常的使用)
  - [exit函数](#exit函数)
- [类型转换](#类型转换)
- [继承和组合](#继承和组合)
- [随机数发生器](#随机数发生器)
- [**待续...**](#待续)
- [动态内存](#动态内存)
- [命名空间](#命名空间)
- [模板](#模板)
- [预处理器](#预处理器)
- [信号处理](#信号处理)
- [多线程](#多线程)
- [常见bug](#常见bug)

<!-- /code_chunk_output -->

## g++编译
    g++ -g -Wall -std=c++11 main.cpp
    
### 常用选项
    -ansi	只支持 ANSI 标准的 C 语法。这一选项将禁止 GNU C 的某些特色， 例如 asm 或 typeof 关键词。
    -c	只编译并生成目标文件。
    -DMACRO	以字符串"1"定义 MACRO 宏。
    -DMACRO=DEFN	以字符串"DEFN"定义 MACRO 宏。
    -E	只运行 C 预编译器。
    -g	生成调试信息。GNU 调试器可利用该信息。
    -IDIRECTORY	指定额外的头文件搜索路径DIRECTORY。
    -LDIRECTORY	指定额外的函数库搜索路径DIRECTORY。
    -lLIBRARY	连接时搜索指定的函数库LIBRARY。
    -m486	针对 486 进行代码优化。
    -o	FILE 生成指定的输出文件。用在生成可执行文件时。
    -O0	不进行优化处理。
    -O	或 -O1 优化生成代码。
    -O2	进一步优化。
    -O3	比 -O2 更进一步优化，包括 inline 函数。
    -shared	生成共享目标文件。通常用在建立共享库时。
    -static	禁止使用共享连接。
    -UMACRO	取消对 MACRO 宏的定义。
    -w	不生成任何警告信息。
    -Wall	生成所有警告信息。  
    
## enum 枚举类型
可以和switch语句很好结合      
1. 声明 enumType 为新的数据类型，称为**枚举**(enumeration) 可用来声明这种类型的变量,就像int a;
Monday等为符号常量，**枚举量**，默认0-6
也可以定义的类型的时候定义变量(还可以顺带初始化)      

        enum enumType {Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday};
        enum enumType {Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday}Weekday = Monday;
      
2. 枚举量赋值给非枚举量，因为枚举量是符号常量，赋值编译器自动把枚举量转换为int
        
        int a = Monday;
        int a = Monday + 1;
3. 非枚举量强制类型转换 赋值给枚举量
        
        Weekday = enumType(2);  #但要注意**不要超出枚举取值范围**，超出了不会报错，但是得不到想要的结果
4. 自定义枚举常量的值， 这里Monday、Wednesday均被定义为1，则Tuesday=2，Thursday、Friday、Saturday、Sunday的值默认分别为2、3、4、5
        
        enum enumType {Monday=1, Tuesday, Wednesday=1, Thursday, Friday, Saturday, Sunday};
注意：指定的值是**整数**,**未被初始化的枚举值的值默认将比其前面的枚举值大1**,**枚举量的值可以相同**        
5. 强类型枚举
传统c++问题：暴露在外层作用域，如果有相同枚举常量不行

        enum Side{Right,Left};
        enum Thing{Wrong,Right};
        
        #强枚举类型 只能使用 Enumeration::VAL1，单独的VAL1没有意义
        enum class Enumeration{
         VAL1,
         VAL2,
        };
        
## 左值 右值
- **左值(lvalue)**：指向内存位置的表达式被称为左值表达式。可以出现在赋值号的左边和右边。 如变量
- **右值(rvalue)**：存储在内存中某些地址的数值。不能对其进行赋值表达式，只能出现在赋值号右边。如数值型字面值 10

## 变量初始化
1. 局部变量：定义时系统不会对其初始化，需要自行初始化
2. 全局变量：系统自动初始化为下列值
- int	    0
- char	    '\0'
- float	    0
- double	0
- pointer	NULL

## 限定符

限定符	含义
- const	类型的对象在程序执行期间不能被修改改变。
- **volatile** 告诉编译器不需要优化volatile声明的变量，让程序可以直接从内存中读取变量。
对于一般的变量编译器会对变量进行优化，将内存中的变量值放在寄存器中以加快读写效率，
比如编译器发现两次从i读取数据代码之间没有对i操作，优化结果就是直接用上次读到的值，(变量是寄存器变量或者端口数据时容易出错)
用了volatile生成的汇编代码就会重新从i的地址读取数据

    
        volatile int i=10;  
        int a = i;  
        ...  
        // 其他代码，并未明确告诉编译器，对 i 进行过操作  
        int b = i;  
        
## 存储类
1. **auto** 声明变量时根据初始化表达式自动推断该变量的类型、声明函数时函数返回值的占位符。使用少c++11已删除
2. **register** 变量**可能**在寄存器中，取决于硬件和实现的限制。如果在寄存器中，则不能进行**一元&**运算，因为没有内存位置。用于需要快速访问的量
3. **static** 修饰全局变量：作用域限制在声明它的文件内。 局部变量：保持局部变量的值，只初始化一次
        
   1. static数据成员存储在程序静态存储区，独立于该类的任意对象，类对象和类都可以调用
   2. static数据成员必须在**类外初始化**，不能在类中
   3. static const int 可以在类定义内部初始化 其他string double 不可以
4. **extern** 提供一个全局变量引用，使用 'extern' 时，对于无法初始化的变量，会把变量名指向一个之前定义过的存储位置。通常用于多个文件夹共享全局变量
5. **mutable** 使用类对象，mutable成员可以通过const成员函数修改
6. **thread_local** 使用 thread_local 说明符声明的变量仅可在它在其上创建的线程上访问。 变量在创建线程时创建，并在销毁线程时销毁。 每个线程都有其自己的变量副本。

## 运算符优先级
~ : 二进制补码运算
![运算符优先级](运算符优先级.png)

## lambda 函数与表达式
匿名函数
    
    [capture](parameters)->return-type{body}
在Lambda表达式内可以访问当前作用域的变量，这是Lambda表达式的闭包（Closure）行为。可以通过前面的[]来指定：
    
        []      // 沒有定义任何变量。使用未定义变量会引发错误。
        [x, &y] // x以传值方式传入（默认），y以引用方式传入。
        [&]     // 任何被使用到的外部变量都隐式地以引用方式加以引用。
        [=]     // 任何被使用到的外部变量都隐式地以传值方式加以引用。 一旦传值 就是常量
        [&, x]  // x显式地以传值方式加以引用。其余变量以引用方式加以引用。
        [=, &z] // z显式地以引用方式加以引用。其余变量以传值方式加以引用。
        
        //常值传递 copy一份 之后不变
        int a=10;
        auto func2 = [&]{return a + 1;};
        cout << func1() << endl;
        a++;
        cout << func1() << endl;
          
        //mutable更改变量值
        int a=10;
        cout << [=]()mutable{return ++a;}() << endl;  //总是一个const函数，不能在函数体内修改非静态成员变量,这里是修改拷贝的参数,参数本身没有改变
    
1. 捕捉器，按值传递，相当于在表达式定义那一刻，表达式内就copy了一份变量，而且**值永远不变**，即使外部变量变了，这也不变
2. 想在表达式内部修改变量的值，添加**mutable** 

### qt中的lambda表达式
 

    QObject::connect(&newspaper, static_cast<void (Newspaper:: *)(const QString &)>(&Newspaper::newPaper),
                    [=](const QString &name) 
                    { /* Your code here. */ }
                    );
这里实际函数调用的参数是从信号过来的所以不需要再赋值,下面就是调用匿名函数,a赋值为4

    [](int a){cout << a}(4)                    


   
## 指向指针的指针
    //函数如果要改变值就传入指针或这引用,要改变指针就得传入指针的指针,因为在函数参数传递是调用拷贝构造函数进行复制
    int **var;   **var
     
## 虚继承
菱形继承 子类多重拷贝：浪费空间，二义性
class B C 都继承自class A， 而class D继承自 B和C
    
    class A {virtual ~A()}    // 有的析构函数不是虚函数
    class B : virtual public A;
    class C : virtual public A;
    class D : public B, public A;
B C继承了A的数据成员和指向A的指针
D继承了B C的数据成员和分别指向B C的指针

## 运算符重载
    classname operator++();   //前缀自增
    classname operator++(int);  //后缀自增 加了int形参区分，形参是0，但在函数体内用不到
->类成员访问运算符重载 返回类型必须是指针或者类对象 
    
    class Ptr{
       //...
       X * operator->();
    };
    void f(Ptr p )
    {
       p->m = 10 ; // (p.operator->())->m = 10
    }
    
## 虚函数
= 0 告诉编译器，函数没有主体，下面的虚函数是纯虚函数。

    virtual int area() = 0;  
    
## c++ STL
- Containers(容器)    deque list vector map
- Algorithms(算法)
- iterators(迭代器)    vector<int>::iterator v = vec.begin(); v++; 返回指向向量开头的迭代器   
           
## 文件和流
如果一个数字太大,无法使用 setprecision 指定的有效数位数来打印,则许多系统会以科学表示法的方式打印.
比如setprecision(5), 145678.99 输出 1.4568e+005

为了让浮点输出以固定点或小数点表示法显示,使用流操作符fixed,结合setprecision(2)表示小数点后显示的位数
    
    log_file << std::fixed << std::setprecision(8) << force_sensor_val(0) << std::endl;

### 文件输入输出流
1. fstream ofstream ifstream
    
        void open(const char *filename, ios::openmode mode);   //文件流对象的成员函数
    打开模式
    - ios::app	追加模式。所有写入都追加到文件末尾。
    - ios::ate	文件打开后定位到文件末尾。
    - ios::in	打开文件用于读取。
    - ios::out	打开文件用于写入。
    - ios::trunc	如果该文件已经存在，其内容将在打开文件之前被截断，即把文件长度设为0。
    - ios::binary   二进制方式打开，默认打开为文本方式
    
    常用函数
    
        getline(cin, string);   //当然也可以是 fileOgject从文件 ，cin是从键盘  好用 但是可能读取不到最后一行，如果最后一行没有回车
        fileObject.getline(buf, size);
2. 文件位置 istream成员函数seekg  ostream成员函数seekp
    
        fileObject.seekg(n);    //定位到第n个字节    
        fileObject.seekg(n, ios::cur);    //当前位置后移n字节
        fileObject.seekg(n, ios::end);    //末尾向前n字节
        fileObject.seekg(0, ios::end);    //末尾
3. cin.ignore(),默认参数cin.ignore(1,EOF),完整版本是 cin.ignore(int n, char a)
从输入流 (cin) 中提取字符，提取的字符被忽略 (ignore)，不被使用。
每抛弃一个字符，它都要计数和比较字符：如果计数值达到 n 或者被抛弃的字符是 a，
则 cin.ignore()函数执行终止；否则，它继续等待。
它的一个常用功能就是用来清除以回车结束的输入缓冲区的内容，
消除上一次输入对下一次输入的影响。比如可以这么用：cin.ignore(1024,'\n')，
通常把第一个参数设置得足够大，这样实际上总是只有第二个参数 \n 起作用，
所以这一句就是把回车(包括回车)之前的所以字符从输入缓冲(流)中清除出去。
        
        int a,b;
        cout<<"input a:";
        cin>>a;
        cin.ignore(1024, '\n');
        cout<<"input b:";
        cin>>b;

### 字符串输入输出流
<sstream>直接对内存而不是对文件和标准输出(设备)进行操作

stringstream istringstream   ostringstream
同样可以使用seekp()移动写指针 seekg()重定位读指针
    
    //整数转字符串
    #include <sstream>
    
    std::ostringstream oss;
    int name_suffix = 1;
    oss << name_suffix;
    oss.str();       //返回一个新的string对象，替换内置的stringbuf
    
    // 设置输出精度,cout默认是6位有效数字
    cout.precision(12); //设置为12位有效数字
    printf("%.12f",val);  //小数点后12位有效数字

另一种数字转字符串可参考[【C++】int转换为string的两种方法（to_string、字符串流）](https://blog.csdn.net/chavo0/article/details/51038397)
这是c++11新增的内容
    
    std::to_string(int val)   

字符串转数字

    std::stoi(std::string str);  // c++ 11 标准           
    
## 异常处理
1. 抛出异常
    关键字throw将创建程序抛出对象的拷贝,然后包含throw的函数返回了这个对象(即使函数返回值没有这个对象),
    异常导致程序返回的地方和正常return返回的地方不一样.异常发生之前创建的局部对象也将析构(栈反解)
        
        throw "Division by zero condition!";   //类型为const char* 的异常 catch(const char* msg)
        try{throw x;}catch(T& e){}   //可以抛出任意,包括内置类型 int,double...,后面用相应的类型来捕获.通常抛出异常创建的类    
2. 捕获异常:
    **异常处理器catch必须紧跟try之后,catch只会匹配第一个**,然后系统认为此异常已经处理了,不会继续查找下去,如果还想在抛,就必须throw
        
    如果没有任何一个层次的异常处理器匹配到异常,terminate()函数调用,在<exception>内,
       
        try
        {
           // 保护代码
        }catch( ExceptionName e )    //捕获任何异常  (...)
        {
          // 处理 ExceptionName 异常的代码
          throw;  //可以选择继续上抛,但是要保证这个throw仍然在try语句中(可以是上一层函数的try) 异常对象的所有信息被保留
        }
             
    ![异常层次图](异常图.png)
    ![异常说明](异常表.png)
3. 异常安全
    stack的pop()函数没有返回值,很重要的原因就是stack必须保证异常安全,如果为了得到返回值而调用复制构造函数,函数确抛出异常
    导致没有获得栈顶元素,而且栈定指针下移了一个,这是不安全的,所以分成两步.        
4. assert() 断言语句 用于开发阶段调试 #define NDEBUG 使其在最终发行软件中失效
5. 定义自己的异常 最好从runtime_error或者logic_error继承(exception两个最主要的派生类) 
不要直接从exception继承(没有提供一个参数类型为std::string的构造函数)
        
        class MyError : public runtime_error{
        public:
            MyError(const string& msg = "") : runtime_error(msg){}
        };
        
        int main(){
            try{
                throw MyError("my message");
            } catch (MyError & x){
                cout<< x.what()<<endl;
            }
        } 
### 异常的使用
1. 尽量使用引用来捕获异常
    - 避免不必要的对象拷贝
    - 派生类对象被当做基类对象捕获时,避免对象切割,损失信息
2. 构造函数抛出异常时,必须注意对象内部的指针和它的清理方式
3. 不要在析构函数中抛出异常
    - 析构函数会在抛出其他异常的过程中别调用,避免析构函数抛出异常或者使用可能触发异常的语句(栈反解),导致程序调用teminate()
    - 如果析构函数可能抛出异常,在析构函数内部编写try,catch,必须自己处理异常,不能抛出
4. 避免悬挂指针(野指针)
    构造函数抛出异常,而内部又有指针,因为指针没有析构函数,造成资源无法释放,可以使用智能指针,shared_ptr来处理指向堆内存的指针
5. 处理简单错误尽量不用异常处理,只需要显示消息然后退出程序就可以了,然后把清理工作交给操作系统
    cout<<"error message"<<endl;exit(1);             

### exit函数
    void exit(int value);  // 位于<cstdlib>中
    exit(0);  //表示正常退出 退出整个进程.main函数要求返回值为int,在main中的exit相当于return

## 类型转换
- 转换很有用，但是在某些情况下，它强制编译器把一个数据看成比他实际更大的类型，占用更多空间，可能会破坏其他数据
- 转换就是告诉编译器"忘记类型检查　把它看成其他类型"　编译器不会执行类型检查　引入了漏洞，程序出了问题，首先想到类型转换

查看变量类型
    
    #include <typeinfo>
    std::cout<<typeid(var).name()<<std::endl;

1. C风格

        int a = 200;
        long b = (long)a;
        long c = long(a);  //类似于函数调用
2. c++显式转换
比较容易发现转换的位置，定位bug

    static_cast
        
        int a = 200;
        long b = static_cast<long>(a);  //
        static_cast<void (QSpinBox::*)(int)>(&QSpinBox::valueChanged);  //qt 函数指针
    const_cast   const 到非const volatile 到非volatile
    
        const int i=0;
        int* j = const_cast<int*>(&i);   // 不用转换是不能将赋值给非const指针
        
        volatile int k=0;
        int* u = const_cast<int*>(&k);     
               
## 继承和组合
1. 构造函数初始化列表
针对问题：新类的构造函数没有权利访问子对象的私有数据成员，所以不能直接对他们初始化
        
        //Bar是基类，m是MyType的一个成员对象
        MyType::MyType(int i): Bar(i),m(i+1){//...}
- 自动析构函数调用：虽然常常需要在初始化列表中显式构造函数调用，但是不需要做显式的析构函数调用，因为对于任何类型只有一个析构函数，并且不屈任何参数
- 构造函数调用次序是由成员对象在类中声明的次序决定。如果由　初始化列表的次序确定，则会有两个不同构造函数有两种不同调用构造函数顺序，那么析构函数不知道如何逆序调用。

## 随机数发生器
    #include <cstdlib>
    #include <iostream>
    #include <ctime>
    
    // time(0) 返回一个和时间相关的的数作为随机数种子,如果不设置或者设置一样的数那么运行产生的随机数都一样
    srand(time(0));   
    rand();           // 获取随机数 线性同余法 不是真的随机数 N(j+1) = (A*N(j)+B)(mod M)
    
[线性同余法](https://zh.wikipedia.org/wiki/%E7%B7%9A%E6%80%A7%E5%90%8C%E9%A4%98%E6%96%B9%E6%B3%95) 
                       
## **待续...**
## 动态内存
## 命名空间
## 模板
## 预处理器
## 信号处理
## 多线程

## 常见bug
1. 有返回值的函数一定记得最后有个`return`返回需要的值,**建议有返回值函数先写return语句占位置**
            

# const constexpr
- const：大致意思是说我承诺不改变这个值，主要用于说明接口，这样变量传递给函数就不担心变量会在函数内被修改了,编译器负责确认并执行const的承诺。
- constexpr：大致意思是在编译时求值，主要用于说明常量，作用是允许数据置于只读内存以及提升性能。
    
constexpr的好处：

- 是一种很强的约束，更好地保证程序的正确语义不被破坏。
- 编译器可以在编译期对constexpr的代码进行非常大的优化，比如将用到的constexpr表达式都直接替换成最终结果等。
- 相比宏来说，没有额外的开销，但更安全可靠。

# 遇到的问题
1. 不同vector 的iterator不能混用
    
    迭代器(Iterator)是一个对象，它的工作是遍历并选择序列中的对象，
    它提供了一种访问一个容器(container)对象中的各个元素，
    而又不必暴露该对象内部细节的方法。通过迭代器，
    开发人员不需要了解容器底层的结构，就可以实现对容器的遍历。
    
    指针用来遍历**连续存储结构**,迭代器针对容器(不管底层数据结构是否连续),提供堆容器遍历的方法
    
    下面这种方式出错,不能简单得把iterator当成一个索引      
    
        vector<int>::iterator index_vec_1 = find(vec_1.begin(), vec_1.end(), val);
        //出错
        vector<int>::iterator vec_2_new(vec_2.begin(), index_vec_1);  
        // 正确
        vector<int>::iterator vec_1_new(vec_1.begin(), index_vec_1);
        vector<int>::iterator vec_2_new(vec_2.begin(), vec_1_new.size());
2. /usr/lib64/libstdc++.so.6: error adding symbols: DSO missing from command line
    在链接程序的时候链接-lstdc++(在target_link_libraries中增加 stdc++)         

3. 对象初始化使用{}
    参考[问题Initializing std::tuple from initializer list](https://stackoverflow.com/questions/3413050/initializing-stdtuple-from-initializer-list)

    主要涉及到
    - **initializer_list<T>** is a homogeneous collection (all members must be of the same type, so not relevant for std::tuple)
    
    - **Uniform initialization** is where curly brackets are used in order to construct all kinds of objects; arrays, PODs and classes with constructors. Which also has the benefit of solving the most vexing parse)

            struct A { 
            explicit A(int) {}
            };

            A a0 = 3;
            // Error: conversion from 'int' to non-scalar type 'A' requested

            A a1 = {3}; 
            // Error: converting to 'const A' from initializer list would use 
            // explicit constructor 'A::A(int)'

            A a2(3); // OK C++98 style
            A a3{3}; // OK C++0x Uniform initialization

# 引用
1. [C++之enum枚举量声明、定义、使用与枚举类详解](https://blog.csdn.net/Bruce_0712/article/details/54984371)
2. [C++ 教程](https://www.runoob.com/cplusplus/cpp-tutorial.html)
3. [C++中volatile关键字的使用详解](https://blog.csdn.net/zyx_0604/article/details/80689673)
4. [c++中static数据成员，static成员函数](https://www.cnblogs.com/yyxt/p/4802688.html)
5. [c++中lambda表达式的用法](https://blog.csdn.net/iloveyousunna/article/details/78532398)
6. [C++中虚继承的作用及底层实现原理](https://blog.csdn.net/bxw1992/article/details/77726390)
7. [关于constexpr与const](https://blog.csdn.net/qq_22274565/article/details/78719951)
8. [C++中的const和constexpr](https://www.cnblogs.com/wodehao0808/p/3623590.html)
9. [迭代器(Iterator)](https://blog.csdn.net/Dove_Knowledge/article/details/71023512?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)