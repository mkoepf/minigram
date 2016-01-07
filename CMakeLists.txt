cmake_minimum_required(VERSION 3.0)
project(minigram)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/build)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(SOURCE_FILES main.cpp)
add_executable(minigram ${SOURCE_FILES})


#===[ Unit tests ]=============================================================
# Enabling testing is necessary on this level because otherwise the testrunner
# can't be started from CLion through a custom build target
enable_testing()

set(Boost_USE_STATIC_LIBS On)
find_package(Boost COMPONENTS system filesystem unit_test_framework REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})
#==============================================================================


add_executable(unittest unittest.cpp)
target_link_libraries(unittest ${Boost_LIBRARIES})

set_target_properties(unittest PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/testbin)

add_test(NAME unittest WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/testbin
            COMMAND ${CMAKE_BINARY_DIR}/testbin/unittest)

add_custom_target(UnitTestTarget COMMAND ${CMAKE_MAKE_PROGRAM} test ARGS="")
