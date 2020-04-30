### 下载并安装 CLion

http://toolcloud.huawei.com/toolmall/tooldetails/e1a9cc02681c46908e395438809a8615

免审批申请并下载安装

### 下载并安装  MinGW-w64

- 下载 installer

   https://nchc.dl.sourceforge.net/project/mingw-w64/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/mingw-w64-install.exe

- 执行并安装Package

  执行installer,  选择 Package:   mingw2-gcc-g++ , 点击右上角 Installation菜单进行确认执行.

- 配置环境变量

  添加到PATH环境变量中,  例如:  C:\MinGW\bin

- 测试

  ```powershell
  $ g++ -v
  Using built-in specs.
  COLLECT_GCC=g++
  COLLECT_LTO_WRAPPER=c:/mingw/bin/../libexec/gcc/mingw32/9.2.0/lto-wrapper.exe
  Target: mingw32
  Configured with: ../src/gcc-9.2.0/configure --build=x86_64-pc-linux-gnu --host=mingw32 --target=mingw32 --disable-win32-registry --with-arch=i586 --with-tune=generic --enable-static --enable-shared --enable-threads --enable-languages=c,c++,objc,obj-c++,fortran,ada --with-dwarf2 --disable-sjlj-exceptions --enable-version-specific-runtime-libs --enable-libgomp --disable-libvtv --with-libiconv-prefix=/mingw --with-libintl-prefix=/mingw --enable-libstdcxx-debug --disable-build-format-warnings --prefix=/mingw --with-gmp=/mingw --with-mpfr=/mingw --with-mpc=/mingw --with-isl=/mingw --enable-nls --with-pkgversion='MinGW.org GCC Build-20200227-1'
  Thread model: win32
  gcc version 9.2.0 (MinGW.org GCC Build-20200227-1)
  ```

### 配置CLion

菜单:  File -> Settings -> Build,Execution,Deployment -> Toolchains

添加新的Toolchain, 

Environment:  C:\MinGW  

CMake: Bundled

Debugger: Bundler GDB

Make, C Compiler, C++ Compiler 等保持不动, 通过环境变量PATH即可找到. 

注: Environment 选择安装MinGW的目录.

### 测试

新建工程:   

```powershell
菜单:  File -> New Project -> C++ Executable   
语言:  Language Standard:  C++17
```

编译工程: 

```powershell
菜单:  Build -> Build Project
```

执行:

```powershell
菜单: Run -> Run 'xxxx'               注:  xxxx 为工程名称
```

