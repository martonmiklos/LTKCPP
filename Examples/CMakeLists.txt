project(LtkCppExample)
cmake_minimum_required(VERSION 3.25)

add_executable(example1 example1.cpp)
target_link_libraries(example1 PRIVATE ltkcpp)

install(TARGETS example1 DESTINATION ${CMAKE_INSTALL_BINDIR})
