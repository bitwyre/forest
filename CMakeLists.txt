﻿cmake_minimum_required (VERSION 3.0.0)

project ("forest")

set(CMAKE_CXX_STANDARD 14)

option(BUILD_EXAMPLES OFF)
option(BUILD_TESTS OFF)

if(MSVC)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /STACK:10000000")
endif()

add_library(forest INTERFACE)
target_include_directories(forest INTERFACE ${CMAKE_SOURCE_DIR}/include)

if (BUILD_TESTS)
  add_library(Catch2 INTERFACE)
  target_include_directories(Catch2 INTERFACE ${CMAKE_SOURCE_DIR}/vendor/catchorg/Catch2/single_include)
  enable_testing()
  add_subdirectory(test)
endif()

if (BUILD_EXAMPLES)
  add_subdirectory(example)
endif()

if (BUILD_BENCHMARKS)
  set(BENCHMARK_ENABLE_TESTING OFF CACHE BOOL "Suppressing Google Benchmark's Tests" FORCE)
  add_subdirectory(vendor/google/benchmark)
  add_subdirectory(benchmark)
endif()

install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/ DESTINATION include FILES_MATCHING PATTERN "*.hpp")