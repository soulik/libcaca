set(CACAlib_LIBRARIES "")

find_package(X11)
find_package(OpenGL QUIET)
	
set(CACAlib_src
    driver/conio.c
    driver/gl.c
    driver/ncurses.c
    driver/null.c
    driver/raw.c
    driver/slang.c
    driver/vga.c
    driver/win32.c
    driver/x11.c
    codec/export.c
    codec/import.c
    codec/text.c
	attr.c
	box.c
	caca.c
	caca0.c
	caca_conio.c
	canvas.c
	charset.c
	conic.c
	dirty.c
	dither.c
	event.c
	figfont.c
	file.c
	font.c
	frame.c
	getopt.c
	graphics.c
	legacy.c
	line.c
	prof.c
	string.c
	time.c
	transform.c
	triangle.c
    codec/codec.h
    caca.h
    caca0.h
    caca_conio.h
    caca_debug.h
    caca_internals.h
    caca_prof.h
    caca_stubs.h
    caca_types.h
)

if (WIN32)
	list(APPEND CACAlib_src
		../win32/config.h
	)
	include_directories(../win32)
endif()

if (X11_X11_LIB)
	set(USE_X11 ON CACHE BOOL "Enable support for X11 video driver")
	if (USE_X11)
		list(APPEND CACAlib_LIBRARIES
			${X11_LIBRARIES}
		)
		include_directories( ${X11_INCLUDES})
	endif()
endif()

if (OPENGL_FOUND)
	set(USE_GL ON CACHE BOOL "Enable support for X11 video driver")
	if (USE_GL)
		list(APPEND CACAlib_LIBRARIES
			${OPENGL_LIBRARIES}
		)
		include_directories( ${OPENGL_INCLUDE_DIR})
	endif()
endif()

set(CACAlib_sources
	${CACAlib_src}
)

include_directories(./)
add_definitions(-DCACAlib_EXPORTS)

#set(CACAlib_version_script CACAlib.version)

#IF(UNIX)
#    IF(CMAKE_COMPILER_IS_GNUCC)
#         SET(CMAKE_SHARED_LINKER_FLAGS  "${CMAKE_SHARED_LINKER_FLAGS} ${CMAKE_CXX_COMPILE_OPTIONS_VISIBILITY}hidden -Wl,--version-script=${CACAlib_version_script}")
#    ENDIF(CMAKE_COMPILER_IS_GNUCC)
#
#    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wmissing-declarations")
#ENDIF(UNIX)

add_library(CACAlib SHARED
	${CACAlib_sources}
)

set_property(TARGET CACAlib PROPERTY CXX_STANDARD 11)
set_property(TARGET CACAlib PROPERTY CXX_STANDARD_REQUIRED ON)

set_target_properties(CACAlib
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)

#configure_file(zmq.lua zmq.lua COPYONLY)

target_link_libraries (CACAlib
	${CACAlib_LIBRARIES}
)
