# CMakeLists的编写


在单文件编译时，是比较容易的，但是在复杂的工程中，会存在很多的头文件以及源文件，如果去手动的编译就会很麻烦且复杂
这时就要用到cmake，把重复性的操作简单化，而使用camke的第一步就是编写cmakelists，规定好依赖和源文件
```CMake
cmake_minimum_required(VERSION 3.15) //cmake最低版本

project(name) //规定当前项目的名字


include_directories(${CMAKE_CURRENT_SOURCE_DIR}/includes) //头文件所在的文件夹 如果有多个需分别添加

# 第一个参数是可执行文件的名字 后面是源文件
add_executable(cmake_learn_1 src/cmake_learn.cpp tools/hello.cpp) 
```

[[cmake工作流程.canvas]]

编写好cmakelists后，编译需要两行指令
```
cmake -B build
make -C build
```
之后就编译完成了
==下载cmaketool插件之后，直接按f7就可以编译==

但是更推荐的写法是把定义函数的文件编译成库的形式
```cmakelists
add_library(LibRM SHARED ./librm.cpp) #生成动态库
add_library(LibRM STATIC ./librm.cpp) #生成静态库
```

完整写法如下：
```CMake
cmake_minimum_required(VERSION 3.15)   #声明CMake的最低版本
project(HelloRM) #声明工程名
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include) #指定头文件路径

add_executable(helloRM main.cpp)  #生成目标二进制文件helloRM g++ main.cpp
add_library(LibRM SHARED ./librm.cpp) #生成库文件 相当于g++ librm.cpp

target_link_libraries(helloRM LibRM) #为helloRM链接库文件，生成最后的可执行文件，第一个参数是可执行文件的名称，第二个参数是库的名称
```
##### 静态库和的动态库的区别：
![[Pasted image 20260713212303.png]]

*SHARED* 表示动态库，即需要该库的时候才去调用该库，库本身不在可执行文件中，是大多数时候的写法，当把动态库文件删掉时，可执行文件不能再运行。

*STATIC* 表示静态库，链接时会把库文件直接复制到可执行文件中，缺点是当多次调用时可能会多份冗余拷贝，因为每出现一次库所定义的函数就要复制一回。
##### 关于指定头文件

**1.include_directories（）：**
```
include_directories(/path/to/common/include)
# 指定全局路径
add_library(my_lib1 lib1.cpp)
add_library(my_lib2 lib2.cpp)
# my_lib1 和 my_lib2 都会自动包含 /path/to/common/include
```
一但使用这个命令，生成的库都会包含这个路径
在复杂项目中，可能会出现不必要的依赖和命名冲突风险

2.**target_include_directories():**
```cmake
add_library(my_lib1 lib1.cpp)
# 为 my_lib1 添加私有头文件路径（仅 my_lib1 自身需要）
target_include_directories(my_lib1 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)

add_library(my_lib2 lib2.cpp)
# 为 my_lib2 添加公共头文件路径（my_lib2 自身及其链接者都需要）
target_include_directories(my_lib2 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

# my_app 链接了 my_lib2，会自动继承其 PUBLIC 的包含路径
add_executable(my_app main.cpp)
target_link_libraries(my_app PRIVATE my_lib2)

```
该命令为每个库单独添加头文件路径，避免了冲突和污染问题，可以设置 PUBLIC / PRIVATE ，如果是public的话头文件就会向下传递给可执行文件，反之则不会


以opencv的库为例
``` CMakeLists 
find_package(OpenCV REQUIRED) //寻找对应的包

include_directories(${OpenCV_INCLUDE_DIRS})添加头文件所对应的文件夹

add_executable( opencv src/opencv.cpp)

target_link_libraries(opencv ${OpenCV_LIBS}) //链接库

```

