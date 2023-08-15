set(CMAKE_SKIP_BUILD_RPATH FALSE)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH FALSE)
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")

add_executable(cmsample main.cc)

target_link_libraries(cmsample cmsamplelib)

install(TARGETS cmsample
  RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
