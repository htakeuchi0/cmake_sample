add_library(cmsampleimpl SHARED example.cc)
target_include_directories(cmsampleimpl
  PUBLIC ${PROJECT_SOURCE_DIR}/include
)
