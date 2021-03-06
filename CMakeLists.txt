project(xdg)

cmake_minimum_required(VERSION 2.8)

# We have to manually alter the cxx flags to have a working
# travis-ci build. Its container-based infrastructure only features
# a very old cmake that does not support the more current:
#   set_property(TARGET xdg_test PROPERTY CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(XDG_BUILD_TESTS ON CACHE BOOL "Toggles building of the xdg test suite")

find_package(Boost COMPONENTS filesystem system unit_test_framework REQUIRED)

include_directories(
    .
    ${Boost_INCLUDE_DIRS}
)

add_library(xdg xdg.cpp)
set_property(TARGET xdg PROPERTY CXX_STANDARD 11)
target_link_libraries(xdg ${Boost_LIBRARIES})

if (XDG_BUILD_TESTS)
    enable_testing()
    add_definitions(-DBOOST_TEST_DYN_LINK -DBOOST_TEST_MAIN -DBOOST_TEST_MODULE=xdg)
    add_executable(xdg_test xdg_test.cpp)
    set_property(TARGET xdg_test PROPERTY CXX_STANDARD 11)
    target_link_libraries(xdg_test xdg)

    add_test(xdg_test xdg_test)
endif()
