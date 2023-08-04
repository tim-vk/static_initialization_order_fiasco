cmake_minimum_required(VERSION 3.22.1)
project(playground C CXX)

include_directories(./)

file(GLOB SRC_FILES ./*.cpp)

ADD_LIBRARY(utilities SHARED header.h)
set_target_properties(utilities PROPERTIES
   LINKER_LANGUAGE CXX
   OUTPUT_NAME utilities)

add_executable(
  ${PROJECT_NAME}
  ${SRC_FILES}
)

target_link_libraries(${PROJECT_NAME} utilities)
