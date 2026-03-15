cmake_minimum_required(VERSION 3.28 FATAL_ERROR)

project(bq_websocket VERSION 0.1.0 LANGUAGES C)

set(BUILD_SHARED_LIBS OFF)

add_library(bq_websocket
    ${CMAKE_CURRENT_SOURCE_DIR}/bq_websocket.c
    ${CMAKE_CURRENT_SOURCE_DIR}/bq_websocket.h

    ${CMAKE_CURRENT_SOURCE_DIR}/bq_websocket_platform.c
    ${CMAKE_CURRENT_SOURCE_DIR}/bq_websocket_platform.h)

target_include_directories(bq_websocket PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

if(CMAKE_SYSTEM_NAME MATCHES "Emscripten")
    target_link_options(bq_websocket PUBLIC -lwebsocket.js -s USE_SSL=1)
elseif(WIN32)
    target_link_libraries(bq_websocket PUBLIC ws2_32)
endif()

# only web builds of `bq_websocket` come with built-in ssl support
if(NOT CMAKE_SYSTEM_NAME MATCHES "Emscripten")
    find_package(OpenSSL REQUIRED)
    target_link_libraries(bq_websocket PRIVATE OpenSSL::SSL OpenSSL::Crypto)
    target_compile_definitions(bq_websocket PUBLIC BQWS_PT_USE_OPENSSL=1)
endif()
