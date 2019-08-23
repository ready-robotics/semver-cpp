cmake_minimum_required(VERSION 2.8.3)
project(semver)
set(PACKAGE_INCLUDE_DESTINATION /usr/local/include/${PROJECT_NAME})

# check c++11 / c++0x
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "-std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "-std=c++0x")
else()
    message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler. Suggested solution: update the pkg build-essential ")
endif()

include_directories(include)

## Mark header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.{h,hpp}"
)

#############
## Testing ##
#############
set(boost_installation_prefix /usr/include/boost)
set(BOOST_INCLUDE_DIRS $boost_installation_prefix/include)
add_executable(${PROJECT_NAME}_test test/main.cpp test/semver/version.cpp)
target_include_directories(${PROJECT_NAME}_test PRIVATE ${BOOST_INCLUDE_DIRS})
add_test(NAME test COMMAND ${PROJECT_NAME}_test --log_level=error --detect_memory_leaks=1 --report_level=detailed)
