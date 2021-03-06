#-------------------------------------------------------------------------------------------
# Copyright (C) Electronic Arts Inc.  All rights reserved.
#-------------------------------------------------------------------------------------------

#-------------------------------------------------------------------------------------------
# CMake info
#-------------------------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.1)
project(EATestTest CXX)
include(CTest)

#-------------------------------------------------------------------------------------------
# Defines
#-------------------------------------------------------------------------------------------
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_SCL_SECURE_NO_WARNINGS)
add_definitions(-D_CHAR16T)

#-------------------------------------------------------------------------------------------
# Compiler Flags
#-------------------------------------------------------------------------------------------
set (CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_CURRENT_SOURCE_DIR}/packages/EASTL/scripts/CMake")
include(CommonCppFlags)

#-------------------------------------------------------------------------------------------
# Source files
#-------------------------------------------------------------------------------------------
file(GLOB EATESTTEST_SOURCES "source/*.cpp")
set(SOURCES ${EATESTTEST_SOURCES})


#-------------------------------------------------------------------------------------------
# Executable definition
#-------------------------------------------------------------------------------------------
add_executable(EATestTest ${SOURCES})

#-------------------------------------------------------------------------------------------
# Include directories
#-------------------------------------------------------------------------------------------
target_include_directories(EATestTest PUBLIC include)

#-------------------------------------------------------------------------------------------
# Dependencies 
#-------------------------------------------------------------------------------------------
add_subdirectory(packages/EAAssert)
add_subdirectory(packages/EABase)
add_subdirectory(packages/EAMain)
add_subdirectory(packages/EASTL)
add_subdirectory(packages/EAStdC)
add_subdirectory(packages/EAThread)

target_link_libraries(EATestTest EAAssert)
)
target_link_libraries(EATestTest EAMain)
target_link_libraries(EATestTest EASTL)
target_link_libraries(EATestTest EAStdC)
target_link_libraries(EATestTest EATest)
target_link_libraries(EATestTest EAThread)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

if((NOT APPLE) AND (NOT WIN32))
    target_link_libraries(EATestTest ${EASTLTest_Libraries} Threads::Threads rt)
else()
    target_link_libraries(EATestTest ${EASTLTest_Libraries} Threads::Threads)
endif()

#-------------------------------------------------------------------------------------------
# Run Unit tests and verify the results.
#-------------------------------------------------------------------------------------------
add_test(EATestTestRuns EATestTest)
set_tests_properties (EATestTestRuns PROPERTIES PASS_REGULAR_EXPRESSION "RETURNCODE=0")

