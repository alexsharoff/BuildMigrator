cmake_minimum_required(VERSION 3.13)

project(PROJECT C CXX)
enable_language(ASM_MASM)
enable_language(ASM_NASM)
enable_language(RC)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR})
include(extensions)

set(SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}" CACHE PATH "")
set(CMAKE_ASM_YASM_OBJECT_FORMAT win32)
set(CMAKE_ASM_NASM_OBJECT_FORMAT win32)
add_compile_options(-DWIN32=1)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/include_a)
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include_a)
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include_b)

add_executable(app1 ${SOURCE_DIR}/cpp3.cpp ${SOURCE_DIR}/c3.c)

set_source_files_properties(${SOURCE_DIR}/masm1.asm PROPERTIES LANGUAGE ASM_MASM)
set_source_files_properties(${SOURCE_DIR}/nasm1.asm PROPERTIES LANGUAGE ASM_NASM)
set_source_files_properties(${SOURCE_DIR}/yasm1.asm PROPERTIES COMPILE_OPTIONS -DF1)
add_library(shared1 SHARED
    ${SOURCE_DIR}/nasm1.asm
    ${SOURCE_DIR}/masm1.asm
    ${SOURCE_DIR}/c1.c
    ${SOURCE_DIR}/cpp1.cpp
    ${SOURCE_DIR}/rc1.rc
)
target_language_compile_options(shared1 ASM_MASM PRIVATE -DF5)
target_language_compile_options(shared1 ASM_NASM PRIVATE -DF3)
target_language_compile_options(shared1 C PRIVATE -DF7)
target_language_compile_options(shared1 CXX PRIVATE -DF9)
target_language_compile_options(shared1 RC PRIVATE -DF11)
target_language_include_directories(shared1 ASM_MASM PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)
target_language_include_directories(shared1 ASM_NASM PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_language_include_directories(shared1 C PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)
target_language_include_directories(shared1 RC PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)
target_yasm_sources(shared1 PRIVATE ${SOURCE_DIR}/yasm1.asm)

set_source_files_properties(${SOURCE_DIR}/masm2.asm PROPERTIES LANGUAGE ASM_MASM)
set_source_files_properties(${SOURCE_DIR}/nasm2.asm PROPERTIES LANGUAGE ASM_NASM)
set_source_files_properties(${SOURCE_DIR}/yasm2.asm PROPERTIES COMPILE_OPTIONS -DF2)
set_source_files_properties(${SOURCE_DIR}/yasm2.asm PROPERTIES INCLUDE_DIRECTORIES
    "${CMAKE_CURRENT_BINARY_DIR}/include_b;${CMAKE_CURRENT_BINARY_DIR}"
)
add_executable(app2
    ${SOURCE_DIR}/nasm2.asm
    ${SOURCE_DIR}/masm2.asm
    ${SOURCE_DIR}/c2.c
    ${SOURCE_DIR}/cpp2.cpp
    ${SOURCE_DIR}/rc2.rc
)
target_language_compile_options(app2 ASM_MASM PRIVATE -DF6)
target_language_compile_options(app2 ASM_NASM PRIVATE -DF4)
target_language_compile_options(app2 C PRIVATE -DF8)
target_language_compile_options(app2 CXX PRIVATE -DF10)
target_language_compile_options(app2 RC PRIVATE -DF12)
target_language_include_directories(app2 ASM_MASM PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_language_include_directories(app2 ASM_NASM PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)
target_language_include_directories(app2 C PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_language_include_directories(app2 CXX PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include_b)
target_language_include_directories(app2 RC PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
target_yasm_sources(app2 PRIVATE ${SOURCE_DIR}/yasm2.asm)

add_library(shared2 SHARED ${SOURCE_DIR}/c4.c)
target_compile_options(shared2 PRIVATE -DF13)
target_include_directories(shared2 PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
