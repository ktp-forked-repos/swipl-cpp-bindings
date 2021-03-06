cmake_minimum_required(VERSION 2.6)

project(swipl_cpp)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/modules/")

include(CTest)
include(SPCFindCPP11)
include(SPCFindSWIProlog)

if(NOT SWIPL_EXISTS)
  message(FATAL_ERROR "Could not find the 'swipl' command on your system, please make sure SWI-Prolog is correctly installed and try again")
endif()

include_directories("${SPC_SWIPL_INCLUDE_DIRS}")

add_definitions("${SPC_SWIPL_DEFS}" -DSPC_PROLOG_DEFS_DIR="${CMAKE_SOURCE_DIR}/prolog")
link_directories("${SPC_SWIPL_LINK_DIR}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${SPC_CPP11_FLAGS} ${SPC_SWIPL_CXX_FLAGS} -Wall")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
add_library(swiplcpp SHARED src/PrologInterface.cpp src/LazyTermBuilders.cpp)
target_link_libraries(swiplcpp swipl)

install(TARGETS swiplcpp DESTINATION lib)
install(FILES include/PrologInterface.h DESTINATION include)
install(FILES
  cmake/modules/SPCFindCPP11.cmake cmake/modules/SPCFindSWIProlog.cmake
  DESTINATION cmake/modules)

add_subdirectory("ext/gtest/")
add_subdirectory("ext/gmock/")
add_subdirectory("tests/")

