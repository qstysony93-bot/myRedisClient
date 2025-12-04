cmake_minimum_required(VERSION 4.0)
project(myRedisClient)

set(CMAKE_CXX_STANDARD 20)

set(HOMEBREW_PREFIX "/opt/homebrew")
set(OPENSSL_ROOT_DIR "/opt/homebrew/opt/openssl@3")  # Apple Silicon

find_package(OpenSSL REQUIRED)
find_package(Protobuf REQUIRED)
find_package(gflags REQUIRED)

set(WITH_GLOG OFF CACHE BOOL "Disable glog")
set(BUILD_UNIT_TESTS OFF CACHE BOOL "Disable tests")
set(BUILD_BRPC_TOOLS OFF CACHE BOOL "Disable tools")

add_subdirectory(third_party/brpc)

include_directories(
        "/opt/homebrew/include"
        ${CMAKE_SOURCE_DIR}/third_party/brpc/src
        ${OPENSSL_INCLUDE_DIR}
        ${PROTOBUF_INCLUDE_DIRS}
)

link_directories("/opt/homebrew/lib")

add_executable(myRedisClient main.cpp)

target_link_libraries(myRedisClient
        brpc-static
        ${OPENSSL_LIBRARIES}
        ${PROTOBUF_LIBRARIES}
        gflags
        leveldb
        z
        pthread
)

target_link_libraries(myRedisClient "-framework CoreFoundation" "-framework Security")

