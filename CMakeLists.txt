cmake_minimum_required(VERSION 3.8)
project(selene VERSION 0.1.1 LANGUAGES CXX)

option(SELENE_BUILD_TESTS "Build Selene tests" OFF)
option(SELENE_BUILD_EXAMPLES "Build Selene examples" OFF)
option(SELENE_BUILD_BENCHMARKS "Build Selene benchmarks" OFF)
option(SELENE_BUILD_ALL "Build Selene tests, examples, and benchmarks (even with other SELENE_BUILD_* variables OFF)" OFF)

if (SELENE_BUILD_ALL)
    set(SELENE_BUILD_TESTS ON)
    set(SELENE_BUILD_EXAMPLES ON)
    set(SELENE_BUILD_BENCHMARKS ON)
endif()

# Generate compilation database (e.g. for cquery or YouCompleteMe)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(SELENE_DIR ${CMAKE_CURRENT_SOURCE_DIR})

# Conan support: the cmake_paths generator requires Conan >= 1.4.1
if (EXISTS ${CMAKE_BINARY_DIR}/conan_paths.cmake)
    include(${CMAKE_BINARY_DIR}/conan_paths.cmake)
endif()

include(cmake/get_external.cmake)
include(cmake/find_dependencies.cmake)
include(cmake/compiler_options.cmake)

add_subdirectory(selene)

if (SELENE_BUILD_TESTS)
    add_subdirectory(test/selene)
endif()

if (SELENE_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

if (SELENE_BUILD_BENCHMARKS)
    add_subdirectory(benchmark)
endif()
