cmake_minimum_required( VERSION 2.8 )

project( ITACoreLibs )

if( MSVC )
	add_definitions( "/wd4251 /W4 -D_CRT_SECURE_NO_WARNINGS" )
endif()

# example how to set ExternalLibs folder for an entire project (without ENV var)
list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../VistaCMakeCommon" )

# ... or with ENV variable
list( APPEND CMAKE_MODULE_PATH "$ENV{VISTA_CMAKE_COMMON}" )

include( VistaCommon )

# Because submodule require some legacy code, overwrite default settings of ITABase here ...
set( ITA_BASE_WITH_OLD_ATOMICS ON )
set( ITA_BASE_WITH_REGULAR_EXPRESSIONS ON )
set( ITA_BASE_WITH_CONFIG_OLD_IMPL ON )
set( ITA_BASE_WITH_OLD_RAVEN_OPS ON )

# VA
add_subdirectory( ITABase )
add_subdirectory( ITAFFT )
add_subdirectory( ITADataSources )
add_subdirectory( ITAConvolution )
add_subdirectory( ITACTC )
