cmake_minimum_required (VERSION 3.1.0)
project (CACAlib)

include(AC_HEADER_STDC)

add_subdirectory(caca)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY build)

set_target_properties(CACAlib PROPERTIES
	OUTPUT_NAME "caca"
)

if (WIN32)
	set_target_properties(CACAlib PROPERTIES PREFIX "")
endif()
