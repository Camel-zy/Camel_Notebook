---
title: CMake的使用基础
---
做C大程的时候研究了一下cmake的用法
下面是CMakeLists.txt的基础写法
<!-- more -->
以C大程所需的多文件结构为例

主目录下的CMakeLists.txt：

```cmake
# 最低版本号要求
cmake_minimum_required(VERSION 3.0.0)
# 项目名称
project(Text_Editer VERSION 0.1.0)

include(CTest)
enable_testing()

# 包含目录
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include/libgraphics
    ${CMAKE_CURRENT_SOURCE_DIR}/include/simpleGUI
)
aux_source_directory(. SRC_LIST)
# 添加子目录
add_subdirectory(include/libgraphics)
add_subdirectory(include/simpleGUI)
# 生成可执行文件
add_executable(Text_Editer ${SRC_LIST})
# 链接
target_link_libraries(Text_Editer
    libgraphics
    simpleGUI
    FileProcess
    GUI
    TextBox
)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

```

子目录下的CMakeLists.txt：



```cmake
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include/libgraphics
    ${CMAKE_CURRENT_SOURCE_DIR}/include/simpleGUI
)   # 注意如果子目录下的代码也需要include其他库的话 这里也需要include_directories
aux_source_directory(. DIR_LIB_SRCS)
add_library(libgraphics ${DIR_LIB_SRCS})
```



