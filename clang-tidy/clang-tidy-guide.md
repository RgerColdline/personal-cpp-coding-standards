# clang-tidy-guide
---
首先需要澄清：clang-tidy不像clang-format那样方便，配置更麻烦一点，但是我尽量做到让大家开箱即用，至少是一步步来就没问题
提示：需要配合CMakeLists等可以搞出来compile_commands.json的东西使用
clang-tidy是有代码洁癖的东西，如果说你发现一些配置严重干扰了你coding，那么可以发过来讨论，我把它关掉

## 一、为什么需要clang-tidy?
### 🐞 `bugprone-*`
- **作用**：检测易引发逻辑错误、未定义行为或隐蔽缺陷的代码模式。
---
### 🛡️ `cert-*`
- **作用**：实现 [CERT C++ 安全编码标准](https://wiki.sei.cmu.edu/confluence/display/cplusplus)，聚焦安全漏洞预防。
---
### 📘 `cppcoreguidelines-*`
- **作用**：遵循 [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/)（Bjarne Stroustrup 等制定），推动现代、安全、高效 C++ 实践。
---
### 🔄 `modernize-use-nullptr`
- **作用**：**具体检查**。将 `NULL`/`0` 替换为 C++11 `nullptr`，提升类型安全与可读性。
---
### 🔄 `modernize-use-auto`
- **作用**：**具体检查**。在类型冗长或可推导时建议用 `auto`，减少重复、增强可维护性（保留 `const`/引用语义）。
---
### 👁️ `readability-*`
- **作用**：提升代码可读性与风格一致性（通用规则，非绑定特定风格指南）。
---
### ⚡ `performance-*`
- **作用**：识别性能瓶颈或低效模式，提供优化建议。
---
### 🌐 `google-readability-*`
- **作用**：**Google C++ 风格指南专属子集**（属 `google-*` 前缀），强化其 readability 要求。
- **示例**：  
    `google-readability-braces-around-statements`（强制所有控制流加 `{}`）  
    `google-readability-casting`（禁用 C 风格转换，要求 `static_cast`）  
    `google-readability-function-size`（函数行数超限警告）
---
### 🏷️ `readability-identifier-naming`
- **作用**：**具体检查**。校验标识符命名是否符合预设规范（需在 `.clang-tidy` 中配置规则）。
- **示例**：  
    配置后：`int myVar;`（若要求 snake_case）→ 报错建议 `my_var`  
    支持分类规则：类名 `PascalCase`、常量 `kCamelCase`、宏 `UPPER_SNAKE_CASE` 等
---

## 二、下载clang-tidy
---
### Windows
---
以下方法二选一

#### LLVM
如果在clang-format使用说明里面选的是LLVM那么就不用下载安装了

#### MSYS2
```shell
$ pacman -Ss clang-tools-extra
clangarm64/mingw-w64-clang-aarch64-clang-tools-extra
mingw32/mingw-w64-i686-clang-tools-extra
mingw64/mingw-w64-x86_64-clang-tools-extra
ucrt64/mingw-w64-ucrt-x86_64-clang-tools-extra
clang64/mingw-w64-clang-x86_64-clang-tools-extra
```
从上面的命令可以看出来对应的包
我的是ucrt64，那么我应该输入：
```shell
pacman -S mingw-w64-ucrt-x86_64-clang-tools-extra
```

### Ubuntu
---
```shell
sudo apt install clang-tidy-18 # 推荐18+版本（支持C++20）
sudo update-alternatives --install /usr/bin/clang-tidy clang-tidy /usr/bin/clang-tidy-18 100
sudo update-alternatives --set clang-tidy /usr/bin/clang-tidy-18
```

### Arch
---
```shell
sudo pacman -S clang-tools-extra
```
当然和clang-format一样，很大概率已经安装过了

## 三、集成VScode
---
### 0. 前提：
已经有了项目框架，并且使用`CMake`或其他构建工具生成`compile_commands.json`
这里以`CMake`为例：
在`CMakeLists.txt`(包级的`CMakeLists.txt`)合适位置（比较靠下就行）写入
```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```
之后再
```shell
catkin_make # 咱们的ros工程
```
或
```shell
cmake -B build # 正常的工程文件
```
### 1. 构建时检查：
`CMakeLists.txt`里面写上：
```cmake
option(USE_CLANG_TIDY "Use clang-tidy when building" ON)

if(USE_CLANG_TIDY)
  find_program(CLANG_TIDY_EXE clang-tidy)
  if(CLANG_TIDY_EXE)
    set(CMAKE_CXX_CLANG_TIDY
      "${CLANG_TIDY_EXE}"
      "-p=${CMAKE_BINARY_DIR}"
      "-warnings-as-errors=*"
      "--header-filter=.*"
      CACHE STRING "Clang-Tidy command" FORCE
    )
    message(STATUS "✅ Clang-Tidy enabled: ${CLANG_TIDY_EXE}")
  else()
    message(WARNING "⚠️  Clang-Tidy not found. Install with: sudo apt install clang-tidy-18")
  endif()
endif(USE_CLANG_TIDY)
```
其中option后面的ON可以用OFF TRUE FALSE 1 0等表示开关
如需调整配置请告知我
### 2. 编写即用：

1. 在vscode设置里搜索clang-tidy
2. 在用户里勾选上(其实也可以参照具体说明自己选择改，这里只是我推荐)
> C_Cpp › Code Analysis › Clang Tidy › Code Action: Format Fixes （可以选择不开） 
> C_Cpp › Code Analysis › Clang Tidy: Enabled（智能感知是否启用clang-tidy关键开关）
> C_Cpp › Code Analysis › Clang Tidy: Use Build Path

然后再在`C_Cpp › Code Analysis › Clang Tidy: Path`里面写上你的`clang-tidy`可执行程序的路径（Windows一定是完整路径，因为神秘原因，扩展不继承vscode环境变量，不能只写clang-tidy）（linux目前留空即可）
3. .clang-tidy的路径不用填，会自动检测，如果真有需要设置我再看看
4. 在.vscode里面的`c_cpp_properties.json`里面加上
```json
{
	"configuration": [
		"compileCommands": "${workspaceFolder}/build/compile_commands.json"
	]
}
```
在`configuration`里面添加下面那条，并且把路径改成`compile_commands.json`的真实路径

## 其他
---
### 1. 终端中文乱码
`PowerShell`执行`chcp 65001`（UTF-8），或`VSCode`设置中添加 `"terminal.integrated.env.windows": {"CHCP": "65001"}`
