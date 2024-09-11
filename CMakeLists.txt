cmake_minimum_required(VERSION 3.10)
include(cmake/CPM.cmake)
project(sniffer-support)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_EXTENSIONS OFF)
set(BUILD_SHARED_LIBS OFF)

set(BOOST_INCLUDE_LIBRARIES asio endian)
CPMAddPackage(
  NAME Boost
  VERSION 1.86.0
  URL https://github.com/boostorg/boost/releases/download/boost-1.86.0/boost-1.86.0-cmake.7z
  URL_HASH SHA256=ee6e0793b5ec7d13e7181ec05d3b1aaa23615947295080e4b9930324488e078f
  EXCLUDE_FROM_ALL
  OPTIONS
    "BUILD_SHARED_LIBS OFF"
)

CPMAddPackage(NAME lua
    URL https://www.lua.org/ftp/lua-5.4.6.tar.gz
    URL_HASH SHA256=7d5ea1b9cb6aa0b59ca3dde1c6adcb57ef83a1ba8e5432c0ecd06bf439b3ad88 
    VERSION 5.4.6
    DOWNLOAD_ONLY YES
)

set(lua_src_dir ${lua_SOURCE_DIR})
file(GLOB LUA_SRC_FILES CONFIGURE_DEPENDS ${lua_src_dir}/src/*.c)
list(REMOVE_ITEM LUA_SRC_FILES "${lua_src_dir}/src/lua.c" "${lua_src_dir}/src/luac.c")
add_library(radapter_lua STATIC ${LUA_SRC_FILES})
target_include_directories(radapter_lua PUBLIC ${lua_src_dir}/src)
if (UNIX)
    target_compile_definitions(radapter_lua PRIVATE LUA_USE_POSIX)
endif()

CPMAddPackage("gh:p-ranav/argparse@3.1")
CPMAddPackage("gh:fmtlib/fmt#11.0.2")
CPMAddPackage("gh:cyanidle/describe@1.2")
