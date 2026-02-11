# clang-format-guide
---


## 下载clang-format(windows添加系统环境变量)
---
### 如果你下载过了clang编译器
**windows** 问ai（或者参考没有下载过clang编译器的安装方式和找路径方式，来找到clang-format）
**linux** 直接 `which clang-format`
### 没有下载过clang编译器
#### Windows
---
以下下载方式二选一
##### LLVM
###### 下载链接(实际是下载LLVM)
因为直达链接相比从github上手动点掉速掉的有点多我就只写了发布页面
[Github](https://github.com/llvm/llvm-project/releases)
[21.1.0(稳定版)](https://github.com/llvm/llvm-project/releases/download/llvmorg-21.1.0/LLVM-21.1.0-win64.exe)
[22.1.0(预发布版本)](https://github.com/llvm/llvm-project/releases/download/llvmorg-22.1.0-rc2/LLVM-22.1.0-rc2-win64.exe)
###### 较为详细的教学(如果需要)
[2. clang-format 命令行用法 | 明王讲QT](https://www.qt1024.cn/clang-format/2_command)
只用看里面的1

##### MSYS2(如果你已经使用msys2的话)
```shell
# 进msys2的ucrt64终端之后
pacman -S clang

# 找到clang-format，方便加入系统变量和vscode设置clang-format路径
which clang-format
```

#### Ubuntu
---
安装：
```shell
sudo apt install clang-format
```
查看在哪里：
```shell
which clang-format
```

#### Arch
---
安装：
```shell
sudo pacman -S clang
```
~~居然不能单独下载clang-format吗~~

查看在哪里：
**同Ubuntu**


## 集成vscode
---
[9. IDE 集成 clang-format（Qt Creator 和 VSCode） | 明王讲QT](https://www.qt1024.cn/clang-format/9_clangIDE)
直接看里面的2

## 使用format需要知道的事

### 1. .h格式化
由于.h在C语言就有了，在格式化.h时，clang-format会错误地用C标准格式化
此时需要在任何一行有效代码前（文件最开头）加上
```cpp
// clang-format Language: Cpp
```
### 2. 临时禁用
```cpp
    // clang-format off
    long  b=20;
    // clang-format on

    std::cout<<"format\n";

    /* clang-format off */
    std::cout<<"no format\n";
    /* clang-format on */
```

## 学习clang-format
1. [Clang-Format Style Options — Clang 23.0.0git documentation](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)
2. [b站别人讲的](https://www.bilibili.com/video/BV1M467BFE8U)