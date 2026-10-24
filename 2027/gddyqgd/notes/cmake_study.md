# CMakeLists.txt 使用笔记

`CMakeLists.txt` 是 CMake 项目的核心配置文件。CMake 通过其中的指令了解项目结构、源文件列表和编译要求。

## 常用指令

### 基础配置

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)
```

- `cmake_minimum_required()`：指定最低 CMake 版本，建议放在文件最前面。
- `project()`：定义项目名称、版本和使用的语言。

### 创建可执行文件和库

```cmake
add_executable(MyApp
    src/main.cpp
    src/utils.cpp
)

add_library(MyLib STATIC
    src/lib/core.cpp
    src/lib/helper.cpp
)
```

`add_library()` 可以指定 `STATIC`、`SHARED` 或 `MODULE`。不指定类型时，结果由创建目标之前设置的 `BUILD_SHARED_LIBS` 决定。

### 链接库和头文件路径

```cmake
target_link_libraries(MyApp PRIVATE MyLib)

target_include_directories(MyLib PUBLIC include)
target_include_directories(MyApp PRIVATE src)
```

作用域的含义：

| 作用域 | 当前目标可用 | 依赖当前目标的其他目标可用 |
| --- | :---: | :---: |
| `PRIVATE` | 是 | 否 |
| `PUBLIC` | 是 | 是 |
| `INTERFACE` | 否 | 是 |

### 普通变量

```cmake
set(MY_VAR value)
set(MY_LIST a b c)
```

变量通过 `${变量名}` 引用。后面给出多个值时，它们会组成列表。

### 缓存变量

```cmake
set(MY_OPTION "default" CACHE STRING "选项说明")
```

常用类型包括 `STRING`、`BOOL`、`PATH`、`FILEPATH` 和 `INTERNAL`。需要强制覆盖已有缓存值时，可以在末尾添加 `FORCE`。

### 条件语句

```cmake
if(<条件>)
    # ...
elseif(<条件>)
    # ...
else()
    # ...
endif()
```

### 自定义构建命令

```cmake
add_custom_command(
    TARGET MyApp
    POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E echo "Build finished"
    COMMENT "构建完成"
    VERBATIM
)
```

- `PRE_BUILD`：在目标构建前执行；不同生成器下的行为可能不同。
- `PRE_LINK`：在链接前执行。
- `POST_BUILD`：在目标构建完成后执行。

### 安装规则

```cmake
install(TARGETS MyApp
    RUNTIME DESTINATION bin
)

install(TARGETS MyLib
    ARCHIVE DESTINATION lib
)

install(DIRECTORY include/
    DESTINATION include
)
```

- `RUNTIME`：可执行文件，以及部分平台上的动态库。
- `LIBRARY`：动态库。
- `ARCHIVE`：静态库。
- `INCLUDES DESTINATION`：设置导出目标的头文件接口目录，但不会复制头文件。

## 完整示例

```cmake
# 文件路径：项目根目录/CMakeLists.txt

# 1. 指定 CMake 最低版本
cmake_minimum_required(VERSION 3.10)

# 2. 定义项目名称、版本和语言
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

# 3. 使用 C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 4. 添加静态库
add_library(MyLib STATIC
    src/lib/core.cpp
    src/lib/helper.cpp
)

# 5. 添加可执行文件
add_executable(MyApp
    src/main.cpp
    src/utils.cpp
)

# 6. 设置头文件路径
target_include_directories(MyLib PUBLIC ${PROJECT_SOURCE_DIR}/include)
target_include_directories(MyApp PRIVATE ${PROJECT_SOURCE_DIR}/src)

# 7. 将 MyLib 链接到 MyApp
target_link_libraries(MyApp PRIVATE MyLib)

# 8. 设置安装规则
install(TARGETS MyApp
    RUNTIME DESTINATION bin
)

install(TARGETS MyLib
    ARCHIVE DESTINATION lib
)

install(DIRECTORY include/
    DESTINATION include
)
```

## 项目结构

```text
my_project/                  <-- ${PROJECT_SOURCE_DIR}，项目根目录
├── CMakeLists.txt           <-- CMake 配置入口
├── build/                   <-- 构建目录，通常不提交到代码仓库
├── include/                 <-- 公开头文件
│   └── my_header.h
├── src/                     <-- 源文件和私有头文件
│   ├── main.cpp
│   ├── utils.cpp
│   └── lib/
│       ├── core.cpp
│       └── helper.cpp
└── README.md
```
