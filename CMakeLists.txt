cmake_minimum_required(VERSION 3.28 FATAL_ERROR)

project(bq_websocket VERSION 0.1.0 LANGUAGES C)

set(BUILD_SHARED_LIBS OFF)

add_library(bq_websocket
    ${CMAKE_CURRENT_SOURCE_DIR}/bq_websocket_platform.c
    ${CMAKE_CURRENT_SOURCE_DIR}/bq_websocket_platform.h)
target_include_directories(bq_websocket PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

if(CMAKE_SYSTEM_NAME MATCHES "Emscripten")
    target_link_options(bq_websocket PUBLIC -lwebsocket.js)
elseif(WIN32)
    target_link_libraries(bq_websocket PUBLIC ws2_32)
endif()
