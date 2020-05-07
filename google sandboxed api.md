### google sandboxed api

1. 安装必须的软件:

   ```shell
   # 安装必须的软件
   sudo apt-get install -qy build-essential linux-libc-dev ninja-build   python3 python3-pip libclang-7-dev libcap-dev
   
   # pip3安装 clang
   pip3 install clang
   
   
   ```

2. 升级 cmake 到3.8版本以上

   ```shell
   # 下载 cmake 更高版本, 默认ubuntu 16.04 的 cmake 是 3.5
   # 注意:不要使用 apt remove cmake卸载,因为会卸载之前用camke编译好的包
   # cmake官网:  cmake.org/files
   # 下载 cmake 3.17.2
   cd ~/Downloads
   wget https://cmake.org/files/v3.17/cmake-3.17.2-Linux-x86_64.tar.gz
   # 解压缩
   tar -zxvf cmake-3.17.2*.gz
   # 建立软链接, 覆盖低版本cmake
   sudo ln -sf ~/Downlaods/cmake-3.17.2-Linux-x86_64/bin/* /usr/bin
   # 查看 cmake 版本
   cmake version 3.17.2
   ```

3. 安装最新g++, 以便于支持标准库( ubuntu16.04默认是gg++5.4)

   ```shell
   # 加入源
   sudo add-apt-repository ppa:ubuntu-toolchain-r/test
   sudo apt update
   # 安装g++/gcc
   sudo apt instal gcc-8  g++-8
   # 绑定gcc,g++
   sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 20 --slave /usr/bin/g++ g++ /usr/bin/g++-8
   # 切换
   sudo update-alternatives --config gcc 
   # 验证版本
   g++ --version
   ```

   

4. 安装 clang 替换 g++ (非必须)

   ```shell
   # 安装( ubuntu16.04 最高可到 clang-4.0 , ubuntu 18.x 最高可到 6.0 )
   sudo apt install llvm
   sudo apt install clang-4.0
   # 查看版本
   clang --version
   clang++ --version
   clang version 3.8.0-2ubuntu4 (tags/RELEASE_380/final)
   # 切换 C++/C 编译器
   sudo update-alternatives --config c++
   sudo update-alternatives --config cc
   选择 clang++/clang 作为current choice
   
   
   # 如果有多个 clang 需要切换, 使用如下命令同等处理
   sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-3.8  1  --slave /usr/bin/clang++ clang++  /usr/bin/clang++-3.8
   sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-4.0  1  --slave /usr/bin/clang++ clang++ /usr/bin/clang++-4.0
   # 打印版本
   clang --version
   # 切换版本
   sudo update-alternatives --config clang 
   ```

5. 切换为clang编译器为C++编译器 (非必须)

   ```shell
   # 切换 C 编译器
   sudo update-alternatives --config cc
   选择 clang 作为current choice
   ```

6. 下载 google andboxed api

   ```shell
   # 下载 google sandboxed api
   git clone https://github.com/google/sandboxed-api.git
   # 编译准备
   cd sandboxed-api
   mkdir build
   cd build
   # 编译
   cmake ..
   # 注: 此时应该发现, 使用了clang 作为编译器
   make 
   
   ```

   

7. 