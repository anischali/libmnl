cmake_minimum_required(VERSION 3.16.1)
project("libmnl")

file(GLOB libmnl_HEADERS 
    ${CMAKE_CURRENT_LIST_DIR}/include/*/*.h
    ${CMAKE_CURRENT_LIST_DIR}/src/*.h
    ${CMAKE_CURRENT_LIST_DIR}/*.h
)

file(GLOB libmnl_SOURCES
    ${CMAKE_CURRENT_LIST_DIR}/src/*.c
)
    
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_BINARY_DIR}/
)

add_library(mnl
SHARED            
"${libmnl_SOURCES}"
)

target_precompile_headers(mnl PUBLIC
    "${libmnl_HEADERS}"
)

set(LIBVERSION "2:0:2")

set_target_properties(mnl PROPERTIES
    VERSION "${VERSION}"
    PUBLIC_HEADER "${libmnl_HEADERS}"
    SOVERSION 1
)

add_link_options(
    -Wl,--version-script="${CMAKE_CURRENT_SOURCE_DIR}/src/libmnl.map -version-info $(LIBVERSION)"
)

configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/config.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/config.h
    @ONLY
)

configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/libmnl.pc.in
    ${CMAKE_CURRENT_BINARY_DIR}/libmnl.pc
    @ONLY
)

install(TARGETS mnl
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/libmnl
)

install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/libmnl.pc
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig
)
