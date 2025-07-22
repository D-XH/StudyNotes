# CMake

## 基础

1. `cmake_minimum_required(VERSION <version>)`：cmake最低版本
2. `project(<project_name> [<language>...])`：项目名称和语言
3. `add_executable(<target> <source_files>...)`：创建可执行文件和源文件
4. `add_library(<target> <source_files>...)`：创建库和指定源文件
   1. `add_library(MyLibrary STATIC library.cpp)`
5. `aux_source_directory(dir, var)`：指定源文件文件夹
6. `include_directories(<dirs>...)`：指定`include`路径
7. `target_link_libraries(<target> <libraries>...)`：连接目标文件和其他库
8. `set(<variable> <value>...)`：设置变量和值
9. `add_compile_options(-std=c++11 -Wall -O2)`：编译选项

### 设置

```cmake
# 生成库
add_library(lib_name SHARED ${SRC_LIST})
set_target_properties(lib_name PROPERTIES OUTPUT_NAME "new_lib_name")
set(LIBARARY_OUTPUT_PATH ${path})

# 使用库
find_library(lib_var lib_name ${path})
target_link_libraries(exe ${FUNC_LIB})
```



## 命令行参数

1. `-G "visual studio"`：指定生成器
2. `-DCMAKE_BUILD_TYPE=Release`：构建类型

## 变量

### 项目信息

1. `CMAKE_BUILD_TYPE`：构建类型
2. `PROJECT_NAME`：当前项目名称
3. `CMAKE_PROJECT_NAME`：顶层项目名称
4. `PROJECT_VERSION`：完整版本号（例如 1.2.3）
5. `PROJECT_VERSION_MAJOR`：主版本号（1）
6. `PROJECT_VERSION_MINOR`：次版本号（2）
7. `PROJECT_VERSION_PATCH`：补丁版本号（3）

### 系统信息

1. `CMAKE_SYSTEM_NAME`：操作系统名称（Linux，windows...）
2. `CMAKE_SYSTEM_VERSION`：操作系统版本
3. `CMAKE_SYSTEM_PROCESSOR`：处理器架构
4. `WIN32`：windows平台为True
5. `UNIX`：UNIX-like平台
6. `APPLE`：macOS平台
7. `MSVC`：vs编译器

### 构建配置

1. `CMAKE_BUILD_TYPE`：构建类型（Debug，Release...）
2. `CMAKE_DEBUG_POSTFIX`：debug构建的后缀
3. `CMAKE_RUNTIME_OUTPUT_DIRECTORY`：可执行文件输出目录
4. `CMAKE_LIBRARY_OUTPUT_DIRECTORY`：库文件输出目录
5. `CMAKE_ARCHIVE_OUTPUT_DIRECTORY`：静态库输出目录

### 路径相关

1. `CMAKE_CURRENT_SOURCE_DIR`：当前处理的 CMakeLists.txt 所在目录
2. `CMAKE_CURRENT_BINARY_DIR`：当前目标的构建目录
3. `CMAKE_MODULE_PATH`：CMake 模块查找路径
4. `CMAKE_PREFIX_PATH`：第三方库查找路径

### 编译器和工具链

1. `CMAKE_CXX_COMPILER_ID`：编译器标识 (GNU, Clang, MSVC, etc.)
2. `CMAKE_CXX_COMPILER`：C++ 编译器路径
3. `CMAKE_C_COMPILER`：C 编译器路径
4. `CMAKE_CXX_STANDARD`：C++ 标准版本
5. `CMAKE_CXX_FLAGS`：C++ 编译标志
6. `CMAKE_CXX_FLAGS_DEBUG`：Debug 模式的 C++ 编译标志
7. `CMAKE_CXX_FLAGS_RELEASE`：Release 模式的 C++ 编译标志

### 其他

1. `PROJECT_SOURCE_DIR`：工程根目录
2. `PROJECT_BINARY_DIR`：执行cmake命令的目录
3. `CMAKE_CURRENT_SOURCE_DIR`：当前CMakeLists.txt所在路径
4. `EXECUTABLE_OUTPUT_PATH`：二进制可执行文件存放路径
5. `LIBARARY_OUTPUT_PATH`：库存放路径



## 实践

```cmake
cmake_minimum_required(VERSION 3.7...3.21)

# 当CMake的版本低于3.12时，则CMake将会被设置为当前的版本
if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(MyProject CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(PROJECT_VERSION 1.0.0)

set(LIBRARY_OUTPUT_PATH ${proj_path}/lib)
include_directories(${proj_path}/include ${utils})

add_subdirectory()
```

