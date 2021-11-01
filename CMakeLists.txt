cmake_minimum_required(VERSION 3.8)

project(signals CXX)

enable_testing()

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

file(GLOB_RECURSE TARGET_SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp" "${CMAKE_CURRENT_SOURCE_DIR}/src/*.hpp" "${CMAKE_CURRENT_SOURCE_DIR}/src/*.h")

add_library(signals STATIC ${TARGET_SRC})
target_sources(signals PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/signals.h
                               ${CMAKE_CURRENT_SOURCE_DIR}/src/signals.cpp)
target_include_directories(signals PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)
target_compile_options(signals PUBLIC
    $<$<CXX_COMPILER_ID:GNU>:-Wall>
    $<$<CXX_COMPILER_ID:Clang>:-Wall>
    $<$<CXX_COMPILER_ID:AppleClang>:-Wall>
    $<$<CXX_COMPILER_ID:MSVC>:/W4>)

find_package (Threads)
target_link_libraries (signals PUBLIC ${CMAKE_THREAD_LIBS_INIT})

option(SIGNALS_BUILD_TESTS "Build tests, require googletest" ON)
option(SIGNALS_BUILD_DEMO "Build demo" ON)
    
if(SIGNALS_BUILD_TESTS)
add_subdirectory(tests)
endif()
if(SIGNALS_BUILD_DEMO)
add_subdirectory(demo)
endif()
