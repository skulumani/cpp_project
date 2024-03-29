cmake_minimum_required(VERSION 3.9)
project( cpp_project CXX )

# set(CMAKE_CXX_FLAGS "-lpthread -pthread")
# set(CMAKE_CXX_FLAGS_DEBUG "-g -Wall -Wextra -pedantic")
set(CMAKE_CXX_FLAGS_DEBUG "-g -Wall -Wextra -pedantic")
# Add -Weffc++ for all sorts of good errors to fix
set(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG -O3")

################################################################################
# GLOBAL SETUP
################################################################################
set(CMAKE_CXX_STANDARD 11)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
# set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# Find OpenMP
set(ENV{OMP_NUM_THREADS} 4)
set(ENV{OMP_NESTED} TRUE)
find_package(OpenMP REQUIRED)

################################################################################
# GOOGLE TESTING
################################################################################
add_subdirectory(${PROJECT_SOURCE_DIR}/extern/googletest)
ENABLE_TESTING()
include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})

################################################################################
# EIGEN
############################################################################
include_directories(${PROJECT_SOURCE_DIR}/extern/eigen)

################################################################################
# BUILD TARGETS
################################################################################
include_directories(${PROJECT_SOURCE_DIR}/include)
add_executable(main src/main.cpp )

################################################################################
# Testing example
################################################################################
ENABLE_TESTING()

set(test_all_src 
    tests/test_all.cpp
    tests/test_eigen.cpp
    tests/test_gtest.cpp)

add_executable(test_all ${test_all_src})
target_link_libraries(test_all gtest )
add_test(NAME test_all COMMAND test_all
    WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})
