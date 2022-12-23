cmake_minimum_required(VERSION 3.2)
project(spnav VERSION 0.3 LANGUAGES C)

include_directories(${PROJECT_SOURCE_DIR}/include)


option(USE_X11 "Build with X11 support" ON)
option(BUILD_EXAMPLE "Build example programs" ON)


add_library(
    spnav
    SHARED
    src/proto.c src/spnav.c src/util.c src/spnav_magellan.c
)
set_target_properties(
    spnav
    PROPERTIES
    VERSION ${PROJECT_VERSION} SOVERSION ${PROJECT_VERSION_MAJOR}
)
if(USE_X11)
    add_compile_definitions(SPNAV_USE_X11)
    target_link_libraries(spnav PUBLIC X11)
endif()


if(BUILD_EXAMPLE)
    add_executable(
        cube
        examples/cube/cube.c
    )
    target_link_libraries(cube PUBLIC X11 GL GLU spnav)

    add_executable(
        fly
        examples/fly/fly.c
        examples/fly/xwin.c
    )
    target_link_libraries(fly PUBLIC X11 GL GLU spnav m)

    add_compile_definitions(BUILD_X11)
    add_executable(
        simple
        examples/simple/simple.c
    )
    target_link_libraries(simple PUBLIC X11 spnav m)
endif()

set(VERSION ${PROJECT_VERSION})
set(PREFIX ${CMAKE_INSTALL_PREFIX})
set(LIBDIR lib)
configure_file(spacenav.pc.in spacenav.pc @ONLY) 