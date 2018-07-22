cmake_minimum_required(VERSION 3.7)

project(ldd3)

# Make Targets
add_definitions(-std=c11)
add_custom_target(ldd3 COMMAND $(MAKE) -C ${ldd3_SOURCE_DIR} PWD=${ldd3_SOURCE_DIR})

#
# Kernel Modules
#

file(GLOB MOD_SOURCE_FILES "scull*/*.c" "scull*/*.h" "short/*.c" "short/*.h" "misc-modules/*.c")
add_library(dummy_mod ${MOD_SOURCE_FILES})

#set(LINUX_HEADERS_VERSION 4.4.0-83)
#set(LINUX_HEADERS_PATH /usr/src/linux-headers-${LINUX_HEADERS_VERSION})
set(LINUX_DEV_PATH /home/Storage/SSD/Development/Software/eliaskousk/linux)
set(LINUX_PATH ${LINUX_DEV_PATH})
set(LINUX_HEADERS
        ${LINUX_PATH}/arch/x86/include
        ${LINUX_PATH}/arch/x86/include/generated
        ${LINUX_PATH}/arch/x86/include/generated/uapi
        ${LINUX_PATH}/arch/x86/include/uapi
        ${LINUX_PATH}/include
        ${LINUX_PATH}/include/generated
        ${LINUX_PATH}/include/generated/uapi
        ${LINUX_PATH}/include/uapi)
TARGET_INCLUDE_DIRECTORIES(dummy_mod PUBLIC ${LINUX_HEADERS})
TARGET_INCLUDE_DIRECTORIES(dummy_mod PUBLIC include skull short)

TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC __KERNEL__)
TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC MODULE)
TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC CONFIG_X86_64)

TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -nostdinc)
TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -isystem /usr/lib/gcc/x86_64-linux-gnu/5/include)
#TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -include ${LINUX_PATH}/include/linux/kconfig.h)

#
# Misc Programs
#

file(GLOB MISC_SOURCE_FILES "misc-progs/*.c")
add_library(dummy_misc ${MISC_SOURCE_FILES})
