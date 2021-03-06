cmake_minimum_required(VERSION 3.13)

project(PROJECT C CXX)
enable_language(ASM)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR})
include(extensions)

set(SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}" CACHE PATH "")
add_compile_options(-DGCC=1)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/include_a)
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include_a)
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include_b)

add_library(shared SHARED
    ${SOURCE_DIR}/c1.c
    ${SOURCE_DIR}/cpp1.cpp
    ${SOURCE_DIR}/gasm1.S
)
target_language_compile_options(shared ASM PRIVATE -DF5)
target_language_compile_options(shared C PRIVATE -DF1)
target_language_compile_options(shared CXX PRIVATE -DF3)
target_language_include_directories(shared C PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)
target_language_include_directories(shared CXX PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)

add_executable(app1
    ${SOURCE_DIR}/cpp2.cpp
    ${SOURCE_DIR}/c2.c
    ${SOURCE_DIR}/gasm2.S
)
target_language_compile_options(app1 ASM PRIVATE -DF6)
target_language_compile_options(app1 C PRIVATE -DF2)
target_language_compile_options(app1 CXX PRIVATE -DF4)
target_language_include_directories(app1 ASM PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)
target_language_include_directories(app1 C PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_language_include_directories(app1 CXX PRIVATE ${CMAKE_CURRENT_BINARY_DIR})

add_executable(app2 ${SOURCE_DIR}/c3.c)
target_compile_options(app2 PRIVATE -DF7)
