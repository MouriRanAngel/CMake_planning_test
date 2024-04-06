cmake_minimum_required(VERSION 3.29.0)

project(planning)
set(CMAKE_CXX_STANDARD 17)

# Set the output path of the executable file
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

# CMAKE_RUNTIME_OUTPUT_DIRECTORY 构建时放置所有 runtime 目标文件的位置
#
# 构建系统目标的 runtime 输出工件可能是：
# · 由 add_executable() 命令创建的可执行目标的可执行文件 (例如 .exe)
# · 在 DLL 平台上: 由带有 SHARED 选项的 add_library 命令创建的共享库目标的可执行文件 (例如 .dll)
#
# (Linux) CMAKE_LIBRARY_OUTPUT_DIRECTORY
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)

# Add all source files in the ${PROJECT_SOURCE_DIR}/src directory to the SRC_LISTS variable
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LISTS)

# 创建一个共享库 (Shared Library), 名为 common, 并将 ${SRC_LISTS} 中列出的源文件编译到这个库中
add_library(common SHARED ${SRC_LISTS})

# 将 common 库的头文件目录设置为 ${PROJECT_SOURCE_DIR}/include, 使得这个目录下的头文件可以被该库和其他依赖于它的目标使用
target_include_directories(common PUBLIC ${PROJECT_SOURCE_DIR}/include)

# 创建一个可执行文件 planning, 并将 main.cpp 作为其源文件
add_executable(planning main.cpp)

# 将 planning 目标的头文件目录设置为 ${PROJECT_SOURCE_DIR}/include, 使得这个目录下的头文件可以被该可执行文件使用
target_include_directories(planning PUBLIC ${PROJECT_SOURCE_DIR}/include)

# 将 planning 可执行文件链接到 common 库, 使得 planning 可以使用 common 库中定义的功能
target_link_libraries(planning PUBLIC common)
