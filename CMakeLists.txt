cmake_minimum_required(VERSION 3.5.2)

project(MCGoBetween C)
include(ExternalProject)

set(INSTALL_LIB ${CMAKE_SOURCE_DIR}/InBetween/lib)

include_directories(lib)

set ( INSTALL_BIN ${CMAKE_SOURCE_DIR}/InBetween/lib/bin CACHE PATH "Where to install binaries to." )
set ( INSTALL_LIB ${CMAKE_SOURCE_DIR}/InBetween/lib/lib CACHE PATH "Where to install libraries to." )
set ( INSTALL_INC ${CMAKE_SOURCE_DIR}/InBetween/lib/include CACHE PATH "Where to install headers to." )
set ( INSTALL_ETC ${CMAKE_SOURCE_DIR}/InBetween/lib/etc CACHE PATH "Where to store configuration files" )
set ( INSTALL_SHARE ${CMAKE_SOURCE_DIR}/InBetween/lib/share CACHE PATH "Directory for shared data." )

add_subdirectory(lib/lua)

# add_subdirectory(lib/mbedTLS)

ExternalProject_Add(uv_project
  SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/lib/libuv
  CONFIGURE_COMMAND sh ${CMAKE_CURRENT_SOURCE_DIR}/lib/libuv/autogen.sh && ${CMAKE_CURRENT_SOURCE_DIR}/lib/libuv/configure --prefix=${CMAKE_CURRENT_BINARY_DIR}/lib/libuv
  BUILD_COMMAND make
  INSTALL_COMMAND make install
)

set(SOURCE src/main.c)

add_executable(MCGoBetween ${SOURCE})
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/InBetween)

add_dependencies(MCGoBetween lua uv_project)

set(LIB_DIR ${CMAKE_CURRENT_BINARY_DIR}/lib)

link_directories(${LIB_DIR}/lua ${LIB_DIR}/libuv/lib)

target_link_libraries(MCGoBetween
  uv
  liblua
)
