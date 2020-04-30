### C++中调用C的接口

- extern "C" 引入C的库代码

  ```c
  extern "C" void func();
  ```

-  完整的C文件代码

  ```c
  // demo.h
  
  #pragma once
  
  #ifdef __cplusplus
  extern "C" {
  #endif
  
  int testFunction(int a, int b);
  
  
  #ifdef __cplusplus
  }
  #endif
  
  ```
  
```c
  // demo.c
  
  #include "demo.h"
  
  int testFunction(int a, int b) {
  	return  a+b;
  }
  ```
  
注1: 在C++中，为了支持重载机制，在编译生成的汇编码中，要对函数的名字进行一些处理, 加入比如函数的返回类型等等. 而在C中, 只是简单的函数名字而已, 不会加入其他的信息.
  
注2:  `__cplusplus`是C++编译器(如`g++`等)定义的宏;  对于C++编译器, 由于 `__cplusplus` 宏被定义, 因此通过`extern "C"`来通知C++编译器编译后的代码是按照C的obj文件格式编译的，要连接的话按照C的命名规则去找.
  
- C++文件(调用者的代码:

  ```c++
  // main.cpp
  
  #include <iostream>
  #include "demo.h"
  
  int main() {
  	std::cout << "Hello, World!" << std::endl;
  	std::cout << testFunction(1,2) << std::endl;
  
  	return 0;
  }
  ```

### C调用C++的接口

- C语言调用 C++ 基本函数:

  1. 主体思路: 把C++函数声明为`extern "C"`, 以便于供C代码调用C++函数

  2. 已有的C++文件

     ```c++
     // add.cpp
     
     int add(const int a, const int b) {
     	return a+b;
   }
     ```
  
  
  3. 使用另一个C++文件进行包装, 并用 extern "c" 进行包裹
  
     ```c
     // wrap.cpp
     
     int add(const int a, const int b);  
     
     #ifdef __cplusplus
     extern "C"
     {
     #endif
     
     int call_cpp_add(const int a, const int b) {
         return add(a, b);
     }
     
     #ifdef __cplusplus
     }
     #endif
     ```
  
  4. 在C语言文件 main.c 调用
  
     ```c
     // main.c
     
     #include "stdio.h"
     
     int call_cpp_add(const int a, const int b);
     
     int main() {
         printf("add: %d", call_cpp_add(1,2));
     }
     ```
  
- C语言调用 C++ 重载函数:

  1. 主体思路: 在`extern "C"`中的包裹函数中,  把C++多个重载函数分别声明多个不同名函数, 以便于供C代码调用C++函数

  2. 已有的C++文件
  
     ```c++
     // add.cpp
     
     int add(const int a, const int b) {
     	return a+b;
     }
     
     double add(const double a, const double b) {
     	return a+b;
     }
     ```
  
  3. 包装的C++文件, 使用 extern "C"进行包裹
  
     ```c++
     // wrap.cpp
     
     int add(const int, const int );
     double add(const double, const double);
     
     #ifdef __cplusplus
     extern "C"
     {
     #endif
     
     
     int call_cpp_add_int(const int a, const int b) {
     	return add(a, b);
     }
     
     double call_cpp_add_double(const double a, const double b) {
     	return add(a, b);
     }
     
     #ifdef __cplusplus
     }
     #endif
     ```
  
  4. 调用的 main.c 文件
  
     ```c
     // main.c
     
     #include "stdio.h"
     
     int call_cpp_add_int(const int a, const int b);
     double call_cpp_add_double(const double a, const double b);
     
     int main() {
     	printf("add int   : %d\n", call_cpp_add_int(1,2));
     	printf("add double: %f\n", call_cpp_add_double(1.0, 2.0));
     }
     
     ```
  
  ### C语言调用 C++ 对象的成员函数:
  
  1. 主体思路: 在包裹函数中,  创建C++对象, 然后调用对象的成员方法.  方法类似, 就不赘述了.
  
  
  



