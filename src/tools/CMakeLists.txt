# Source codes for tools

set (dump_src dump.c)
set (dump_vertices_src dump.c)
set (convert_src convert.c)
set (sconvert_src sconvert.c)
set (create_ply_src create_ply.c)

include_directories (../lib)

# Make build flags compiler specific for verse_server
if (CMAKE_COMPILER_IS_GNUCC)
    set (CMAKE_C_FLAGS "-D_REETRANT -Wall -Wextra -pedantic -Wno-long-long")
	if (CMAKE_BUILD_TYPE STREQUAL "Debug")
   	    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -ggdb -O0")
	elseif( CMAKE_BUILD_TYPE STREQUAL "Release" )
	    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DNDEBUG -O3 -fno-strict-aliasing")
	endif ()
endif (CMAKE_COMPILER_IS_GNUCC)

# Set up dump_vertices executables
add_executable (dump_vertices ${dump_vertices_src})
add_dependencies (dump_vertices librply_shared_lib)
target_link_libraries (dump_vertices librply_shared_lib )

# Set up dump executables
add_executable (dump ${dump_src})
add_dependencies (dump librply_shared_lib)
target_link_libraries (dump librply_shared_lib )

# Set up convert executables
add_executable (convert ${convert_src})
add_dependencies (convert librply_shared_lib)
target_link_libraries (convert librply_shared_lib )

# Set up convert executables
add_executable (sconvert ${sconvert_src})
add_dependencies (sconvert librply_shared_lib)
target_link_libraries (sconvert librply_shared_lib )

# Set up convert executables
add_executable (create_ply ${create_ply_src})
add_dependencies (create_ply librply_shared_lib)
target_link_libraries (create_ply librply_shared_lib )
