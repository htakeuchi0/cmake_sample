include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.14.0.zip
)

# For Windows: Prevent overriding the parent project's compiler/linker settings
if (WIN32)
  set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
endif()
set(BUILD_GMOCK OFF CACHE BOOL "" FORCE)
set(INSTALL_GTEST OFF CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

add_executable(gtest-${PROJECT_NAME} ${PROJECT_SOURCE_DIR}/src/example.cc ${PROJECT_SOURCE_DIR}/test/gtest_example.cc)
target_link_libraries(gtest-${PROJECT_NAME} PRIVATE gtest_main)
target_include_directories(gtest-${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/include)
add_test(COMMAND gtest-${PROJECT_NAME})
enable_testing()

find_program(GCOV gcov)
if (NOT GCOV)
  message(WARNING "program gcov not found")
endif()

find_program(LCOV lcov)
if (NOT LCOV)
  message(WARNING "program lcov not found")
endif()

if (GCOV AND LCOV)
  set(COVERAGE coverage.info)
  set(LCOVDIR lcov)
  ADD_CUSTOM_TARGET(lcov DEPENDS gtest-${PROJECT_NAME})
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --coverage")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} --coverage")
  ADD_CUSTOM_COMMAND(
    TARGET lcov
    WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/build/test/CMakeFiles/gtest-${PROJECT_NAME}.dir/
    COMMAND ${PROJECT_SOURCE_DIR}/build/test/gtest-${PROJECT_NAME}
    COMMAND lcov --capture --directory . --output-file ${COVERAGE}
    COMMAND lcov --remove ${COVERAGE} **include/c++/** --output-file ${COVERAGE}
    COMMAND lcov --remove ${COVERAGE} **bits** --output-file ${COVERAGE}
    COMMAND lcov --remove ${COVERAGE} **gtest*.h --output-file ${COVERAGE}
    COMMAND lcov --remove ${COVERAGE} **gtest*.cc --output-file ${COVERAGE}
    COMMAND genhtml ${COVERAGE} --output-directory ${PROJECT_SOURCE_DIR}/build/${LCOVDIR}
    COMMAND rm -f *.gcno
    COMMAND rm -f *.gcda
    COMMAND rm -f ${COVERAGE}
    COMMAND echo -n -e ""
    COMMAND echo ${LCOVDIR}/index.html)
endif()
