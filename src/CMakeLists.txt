add_library(csample SHARED example.cc)
target_include_directories(csample
  PUBLIC ${PROJECT_SOURCE_DIR}/include
)
