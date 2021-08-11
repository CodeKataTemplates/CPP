cmake_minimum_required(VERSION 3.12)

project(kata LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  option(KATA_ASAN "Enable Address Sanitizer" ON)
  option(KATA_UBSAN "Enable Undefined Behavior Sanitizer" ON)

  if (KATA_ASAN)
    list(APPEND sanitizers "address")
  endif ()
  if (KATA_UBSAN)
    list(APPEND sanitizers "undefined")
  endif ()
  if (sanitizers)
    list(JOIN sanitizers "," fsanitize)
    string(APPEND CMAKE_CXX_FLAGS " -fsanitize=${fsanitize}")
  endif ()
endif ()

include(CTest)
add_subdirectory(src)
add_subdirectory(test)
