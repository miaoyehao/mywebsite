---
title: "STM32F03标准库cmake|demo"
date: 2025-01-21T7:00:45+08:00
draft: false
---

<style>
.apple-divider {
    border: none;
    height: 1px;
    background: #d2d2d7;
    margin: 2.5rem auto;
    max-width: 980px;
}
</style> 

#### <div style="text-align: center;"> 使用强大的cmake、openocd开源编译器，不限平台。</div>
<hr class="apple-divider">

##### 一、安装必要工具 
1. MAC的安装可以通过homebrew；linux等可以使用自带的包管理器，windows可以使用MINGW。
```bash
#For mac,first install homebrew 
xcode-select --install 
git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git brew-install
/bin/bash brew-install/install.sh
rm -rf brew-install
```
```bash
#Then
brew install cmake make armmbed/formulae/arm-none-eabi-gcc openocd git 
```
##### 二、安装vscode
1. 从[Visual Studio Code官网](https://code.visualstudio.com/)下载并安装vscode
2. 安装必要的vscode插件：
    - C/C++
    - CMake
    - Cortex-Debug
##### 三、系统工程示意图
![系统工程示意图](https://pisces.now.cc/d/BQACAgUAAxkDAAOEZ49h08GanaUAAZKv9RO_g5z4K9lQAAJzFAACJwl4VGL7TMIYHgaGNgQ)
1. CMakeFlis.txt 文件编写
```bash
#THIS FILE IS AUTO GENERATED FROM THE TEMPLATE! DO NOT CHANGE!
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_VERSION 1)
cmake_minimum_required(VERSION 3.20)

# specify cross compilers and tools
set(CMAKE_C_COMPILER arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)
set(CMAKE_ASM_COMPILER arm-none-eabi-gcc)
set(CMAKE_AR arm-none-eabi-ar)
set(CMAKE_OBJCOPY arm-none-eabi-objcopy)
set(CMAKE_OBJDUMP arm-none-eabi-objdump)
set(SIZE arm-none-eabi-size)
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)


# project settings
#project(Project C CXX ASM)
get_filename_component(DIR_NAME ${CMAKE_CURRENT_SOURCE_DIR} NAME)
project(${DIR_NAME} C CXX ASM)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 11)

# Uncomment for hardware floating point
# add_compile_definitions(ARM_MATH_CM4;ARM_MATH_MATRIX_CHECK;ARM_MATH_ROUNDING)
# add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
# add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)

# Uncomment for software floating point
# add_compile_options(-mfloat-abi=soft)

add_compile_options(-mcpu=cortex-m3 -mthumb -mthumb-interwork)
add_compile_options(-ffunction-sections -fdata-sections -fno-common -fmessage-length=0)

# uncomment to mitigate c++17 absolute addresses warnings
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-register")
if ("${CMAKE_BUILD_TYPE}" STREQUAL "Release")
    message(VERBOSE "Maximum optimization for speed")
    add_compile_options(-Ofast)
elseif ("${CMAKE_BUILD_TYPE}" STREQUAL "RelWithDebInfo")
    message(VERBOSE "Maximum optimization for speed, debug info included")
    add_compile_options(-Ofast -g)
elseif ("${CMAKE_BUILD_TYPE}" STREQUAL "MinSizeRel")
    message(VERBOSE "Maximum optimization for size")
    add_compile_options(-Os)
else ()
    message(VERBOSE "Minimal optimization, debug info included")
    add_compile_options(-Og -g)
endif ()

# 添加宏定义
add_definitions(-DUSE_HAL_DRIVER -DSTM32F103xB -DUSE_STDPERIPH_DRIVER -DSTM32F10X_MD)

# 添加头文件路径，即.h文件
include_directories(
    ./Hardware 
    ./Library 
    ./Start 
    ./System 
    ./User
)

# 添加源文件路径,即.c或者.s文件
file(GLOB_RECURSE SOURCES 
    ./Hardware/*.c
    ./Library/*.c
    ./Start/*.c
    ./System/*.c
    ./User/*.c
    ./Start/startup_stm32f103xb.s
)

# 添加你的STM32F103ZETx_FLASH.ld的连接脚本路径
set(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/Start/stm32f103c8tx_flash.ld)

add_link_options(-Wl,-gc-sections,--print-memory-usage,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map)
# 选择cortex-m3内核
add_link_options(-mcpu=cortex-m3 -mthumb -mthumb-interwork)
add_link_options(-T ${LINKER_SCRIPT})

add_link_options(-specs=nano.specs -specs=nosys.specs -u _printf_float)

add_executable(${PROJECT_NAME}.elf ${SOURCES} ${LINKER_SCRIPT})

#set(HEX_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.hex)
#set(BIN_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.bin)

#add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
#    COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
#    COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
#    COMMENT "Building ${HEX_FILE}
#Building ${BIN_FILE}")

```
2. 编写用于生成和烧录程序的脚本run.sh和flash.cfg
```bash
#!/bin/bash
#run.sh脚本
# 获取当前脚本所在目录
SCRIPT_DIR=$(dirname "$(realpath "$0")")
# 从当前目录开始，向上查找直到找到 CMakeLists.txt 文件，确定项目根目录
PROJECT_DIR=$SCRIPT_DIR
while [ ! -f "$PROJECT_DIR/CMakeLists.txt" ]; do
    PROJECT_DIR=$(dirname "$PROJECT_DIR")
done
# 获取项目根目录名
PROJECT_NAME=$(basename "$PROJECT_DIR")
echo "Project root directory name: $PROJECT_NAME"
# 创建并进入构建目录
BUILD_DIR="$PROJECT_DIR/build"
mkdir -p "$BUILD_DIR"
cd "$BUILD_DIR"
# 运行 CMake 配置和编译项目
cmake ..
make -j 16
# 根据项目名称生成 ELF、BIN 和 HEX 文件路径
ELF_FILE="$BUILD_DIR/${PROJECT_NAME}.elf"
BIN_FILE="$BUILD_DIR/${PRoJECT_NAME}.bin"
HEX_FILE="$BUILD_DIR/${PROJECT_NAME}.hex"
# 检查 ELF 文件是否生成成功
if [ -f "$ELF_FILE" ]; then
# 将ELF 文件接換力 BIN 文件和 HEX 文件
    arm-none-eabi-objcopy -O binary "$ELF_FILE" "$BIN_FILE" 
    arm-none-eabi-objcopy -O ihex "$ELF_FILE" "$HEX_FILE"
    echo "Conversion to BIN and HEX completed."
else
    echo "Error: ELF file not found. Compilation might have failed."
    exit 1
fi
# 执行 OpenOCD 进行烧录
openocd -f "$PROJECT_DIR/flash.cfg"
# uncomment this could support downloading code to flash
```
```bash
#flash.cfg, 注意你的烧录工具和程序文件路径
source [find interface/stlink.cfg]
source [find target/stm32f1x.cfg]
program /Users/miao/Documents/StmProject/Test/build/Test.hex verify reset exit
```