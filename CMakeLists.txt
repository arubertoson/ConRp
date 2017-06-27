cmake_minimum_required(VERSION 3.6 FATAL_ERROR)
project(ConRp)

# add_definitions(
#     -std=c++14
#     -w
# )
# pthreads is needed for chrono to work as expected
set(CMAKE_CXX_FLAGS "-std=c++14 -w -pthread")
set(CMAKE_CXX_CLANG_TIDY "clang-tidy" "-checks=*,-google-runtime-int")
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

set(CMAKE_SHARED_LIBRARY_LINK_C_FLAGS "")
set(CMAKE_SHARED_LIBRARY_LINK_CXX_FLAGS "")

file(GLOB_RECURSE HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/src/*.h)
file(GLOB_RECURSE SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
)
list(REMOVE_ITEM SOURCES ConRp.cpp)

# message(STATUS "SOURCES" ${})
include_directories(src)

add_library(${PROJECT_NAME}_lib ${SOURCES})

add_executable(${PROJECT_NAME} src/ConRp.cpp)
target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}_lib)


# TEST
enable_testing()
add_library(CATCH INTERFACE)
find_path(CATCH_INCLUDE_DIR "catch.hpp" PATHS "test/catch")
target_include_directories(CATCH INTERFACE ${CATCH_INCLUDE_DIR})


file(GLOB_RECURSE TEST_SOURCES "test/*.cpp")


add_executable(${PROJECT_NAME}_test ${TEST_SOURCES})
target_link_libraries(${PROJECT_NAME}_test ${PROJECT_NAME}_lib)
# add_test(NAME ${} COMMAND)

