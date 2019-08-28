cmake_minimum_required(VERSION 2.8.3)
project(semver)

# Build Options
set(BUILD_TESTS False CACHE BOOL "Build with 'make test' option")
set(PACKAGE_INCLUDE_DESTINATION /usr/local CACHE STRING "Include file install location")
set(BOOST_INCLUDE_DIRS /usr/include/boost/include CACHE STRING "Boost include directory location")

# Check C++11 / C++0x support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
  set(CMAKE_CXX_FLAGS "-std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
  set(CMAKE_CXX_FLAGS "-std=c++0x")
else()
  message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++ support. Please use a different C++ compiler. Suggested solution: update the pkg build-essential ")
endif()

include_directories(include)

# Mark header files for installation
install(
  DIRECTORY include
  DESTINATION ${PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.hpp"
)

# Testing
if(BUILD_TESTS)
  enable_testing()
  add_executable(${PROJECT_NAME}_test test/main.cpp test/semver/version.cpp)
  target_include_directories(${PROJECT_NAME}_test PRIVATE ${BOOST_INCLUDE_DIRS})
  add_test(NAME version_test COMMAND ${PROJECT_NAME}_test --log_level=error --detect_memory_leaks=1 --report_level=detailed)
endif()
