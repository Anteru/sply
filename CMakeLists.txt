cmake_minimum_required(VERSION 2.8.12)
project(SPly)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_library(SPly STATIC sply.c sply.h)

option(SPLY_BUILD_UTILITIES "Build the SPly utilities" OFF)

if(SPLY_BUILD_UTILITIES)
    add_executable(dump etc/dump.c)
    target_link_libraries(dump SPly)

    add_executable(convert etc/convert.c)
    target_link_libraries(convert SPly)

    add_executable(sconvert etc/sconvert.c)
    target_link_libraries(sconvert SPly)
endif(SPLY_BUILD_UTILITIES)
