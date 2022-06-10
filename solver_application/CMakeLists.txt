
cmake_minimum_required(VERSION 3.4)
project(equation)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
        ${CMAKE_CURRENT_BINARY_DIR}/formatter_ex)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib
        ${CMAKE_CURRENT_BINARY_DIR}/solver)
add_executable(equation ${CMAKE_CURRENT_SOURCE_DIR}/equation.cpp)
target_link_libraries(equation formatter_ex solver)
