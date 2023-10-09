cmake_minimum_required(VERSION 3.10)

project(qtwebapp LANGUAGES CXX)

find_program(CCACHE_FOUND ccache)
if(CCACHE_FOUND)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ccache)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ccache) # Less useful to do it for linking, see edit2
endif(CCACHE_FOUND)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "-Wall -Wextra -Wpedantic ${CMAKE_CXX_FLAGS}")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 ${CMAKE_CXX_FLAGS}")
set(CMAKE_CXX_FLAGS_DEBUG "-g -O0 ${CMAKE_CXX_FLAGS}" )
set(CMAKE_BUILD_PARALLEL_LEVEL 16)

set(CMAKE_AUTOMOC ON)

find_package(Qt5 REQUIRED COMPONENTS Core Network)

set(TEMPLATE_DIR   ${PROJECT_SOURCE_DIR}/templateengine)
set(LOGGING_DIR    ${PROJECT_SOURCE_DIR}/logging)
set(HTTPSERVER_DIR ${PROJECT_SOURCE_DIR}/httpserver)

file(GLOB SRC_TEMPLATE   ${TEMPLATE_DIR}/*.cpp ${TEMPLATE_DIR}/*.h)
file(GLOB SRC_LOGGING    ${LOGGING_DIR}/*.cpp ${LOGGING_DIR}/*.h)
file(GLOB SRC_HTTPSERVER ${HTTPSERVER_DIR}/*.cpp ${HTTPSERVER_DIR}/*.h)
set(SRC ${SRC_TEMPLATE} ${SRC_LOGGING} ${SRC_HTTPSERVER})

add_library(${PROJECT_NAME} ${SRC})
target_include_directories(${PROJECT_NAME} PUBLIC ${TEMPLATE_DIR} ${LOGGING_DIR} ${HTTPSERVER_DIR})
target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Core Qt5::Network)
