### Bazel
1. install bazel
   ```shell
   # 安装所需的包
   $ sudo atp install unzip zip g++-9

   # download 
   $ wget https://github.com/bazelbuild/bazel/releases/download/3.1.0/bazel-3.1.0-installer-linux-x86_64.sh
   
   # install
   $ chmod + bazel-*linux-x86_64.sh
   # --user 安装到 $HOME/bin 下
   $ ./bazel-*linux-x86_64.sh --user

   # environment 添加到 ~/.bashrc 中
   export PATH="$PATH:$HOME/bin"

   ```
   
2. Bazel 基本概念: workspace
   ```shell
   # 工作区: 存放所有源代码和Bazel编译输出文件的目录, 是整个项目的根目录
   # 一个或多个BUILD文件, 用于构建项目的不同部分
   # 工作区根目录必须包含一个名为 WORKSPACE 的文件, 可以空, 或包含应用外部构建所需的依赖关系

   ```

3. Bazel 基本概念: Build文件
   ```shell
   # 一个Build文件包含了集中不同类型的指令, 例如编译指令, 告诉Bazel如何编译二进制文件或库;
   # Build文件中每一条编译指令被称为一个target, 指向一系列的源文件和依赖或另一个target; 
   cc_binary (
       name="hello-world",
       srcs = ["hello-world.cc"],
   )
   # 可通过执行 bazel 来编译这个样例
   bazel build //main:hello-world
   
   ```

4. 构建一个Bazel项目
    ```shell
    # 目录结构:
    $ tree
    .
    ├── main
    │   ├── BUILD
    │   ├── hello-greet.cc
    │   ├── hello-greet.h
    │   └── hello-world.cc
    └── WORKSPACE
    ```
    ```c++
    // main/hello-world.cc 文件内容
    
#include <iostream>
    #include "hello-greet.h"
    
    
    
    int main(int argc, char** argv){
        std::string who = "bazel";
        if( argc > 1) {
            who = argv[1];
    }
        std::cout << get_greet(who) << std::endl;
    return 0;
    }
    
    ```
    ```c++
    // hello-greet.h 文件
    
    #pragma once
    
    #include <string>
    
    std::string get_greet(const std::string &thing);
    ```
    
    ```c++
    // hello-greet.cc 文件
    
    #include "hello-greet.h"
    #include <string>
    
    std::string get_greet(const std::string &who) {
        return "hello " + who;
    }
    
    ```
    
    
    
    ```c++
    // main/BUILD 文件
    
    load("@rules_cc//cc:defs.bzl", "cc_binary")
    
    cc_library(
        name = "hello-greet",
        srcs = ["hello-greet.cc"],
        hdrs = ["hello-greet.h"],
    
    )
    
    cc_binary(
        name = "hello-world",
        srcs = ["hello-world.cc"],
        deps = [
            ":hello-greet",
        ]
    )
    ```
    
5. 执行编译
   ```shell
   # 编译
   #   其中 //main: 是 BUILD 文件相对于 WORKSPACE 文件的位置, hello-world 是BUILD文件中命名好的 target 名字
   bazel build //main:hello-world
   # 执行可执行
   #   输出的可执行文件在 bazel-bin 下
   bazel-并/main/hello-world 
   ```



   