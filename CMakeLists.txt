cmake_minimum_required(VERSION 3.0)
project(cmake_sample VERSION 0.0.1 LANGUAGES CXX)

add_subdirectory(src)
add_subdirectory(test)
add_subdirectory(main)
add_subdirectory(include)

find_program(DOXYGEN doxygen)
if (NOT DOXYGEN)
  message(WARNING "program doxygen not found")
endif()

ADD_CUSTOM_TARGET(docs)
ADD_CUSTOM_COMMAND(
  TARGET docs
  WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
  COMMAND doxygen
  COMMAND mv doxygen/ build/)
