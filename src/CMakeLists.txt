add_library(cmsamplelib SHARED example.cc)

target_include_directories(cmsamplelib PUBLIC
  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

install(TARGETS cmsamplelib
  EXPORT cmsamplelib-config
  LIBRARY DESTINATION lib)

install(EXPORT cmsamplelib-config
  NAMESPACE cmsamplelib::
  DESTINATION lib/cmake/cmsamplelib)

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/cmsamplelib-config-version.cmake
    COMPATIBILITY SameMajorVersion)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/cmsamplelib-config-version.cmake
    DESTINATION lib/cmake/cmsamplelib)

add_library(cmsaplelib::cmsamplelib ALIAS cmsamplelib)
