cmake_minimum_required( VERSION 2.8 )

project( ITACoreLibs )


# example how to set ExternalLibs folder for an entire project (without ENV var)
list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../VistaCMakeCommon" )

# ... or with ENV variable
list( APPEND CMAKE_MODULE_PATH "$ENV{VISTA_CMAKE_COMMON}" )

include( VistaCommon )

# Some global settings for static/shared library generation
if( NOT DEFINED ITA_VISTA_BUILD_STATIC )
	set( ITA_VISTA_BUILD_STATIC OFF CACHE BOOL "Build against static ViSTA libraries" )
endif( )

# Apps and tests
if( NOT DEFINED ITA_CORE_LIBS_WITH_APPS )
	set( ITA_CORE_LIBS_WITH_APPS ON CACHE BOOL "Build ITACoreLibs applications" )
endif()
if( NOT DEFINED ITA_CORE_LIBS_WITH_TESTS )
	set( ITA_CORE_LIBS_WITH_TESTS OFF CACHE BOOL "Build ITACoreLibs tests" )
endif( )

# Because submodules (and RAVEN) require some legacy code, overwrite default settings of ITABase
set( ITA_BASE_WITH_OLD_ATOMICS ON CACHE BOOL "Build with old atomic code for non-C++11 compatible compilers" )
set( ITA_BASE_WITH_REGULAR_EXPRESSIONS ON CACHE BOOL "Build with old regular expressions code for string manipulation" )
set( ITA_BASE_WITH_CONFIG_OLD_IMPL ON CACHE BOOL "Build with old INI file configuration implementation (uses legacy regular expressions)" )
set( ITA_BASE_WITH_OLD_RAVEN_OPS ON CACHE BOOL "Build with old RavenOps helper functions implementation (legacy code for RAVEN compatibility)" )


# Compiler
if( MSVC )
	add_definitions( "/wd4251 /W4 -D_CRT_SECURE_NO_WARNINGS" )
endif()

# ITACoreLibs submodules
set( ITA_CORE_LIBS_COMMON_BUILD TRUE )
add_subdirectory( ITABase )
add_subdirectory( ITAFFT )
add_subdirectory( ITAStreaming )
add_subdirectory( ITADataSources )
add_subdirectory( ITAConvolution )
add_subdirectory( ITADSP )
add_subdirectory( ITACTC )
add_subdirectory( ITASampler )

# Project
if( MSVC )
	set( ITA_CORE_LIBS_INCLUDE_EXTERNAL_MSVC_PROJECTS OFF CACHE BOOL "If enabled, msvc projects of external vista packages will be included in the solution" )
	if( ITA_CORE_LIBS_INCLUDE_EXTERNAL_MSVC_PROJECTS )
		vista_use_package( VistaCoreLibs )
		vista_add_external_msvc_project_of_package( VistaCoreLibs "VistaCoreLibs" DEPENDENT ITABase )
		vista_add_external_msvc_project_of_package( VistaCoreLibs "VistaCoreLibs" DEPENDENT ITADataSources )
		vista_add_external_msvc_project_of_package( VistaCoreLibs "VistaCoreLibs" DEPENDENT ITAConvolution )
		vista_add_external_msvc_project_of_package( VistaCoreLibs "VistaCoreLibs" DEPENDENT ITACTC )
	endif( )
endif( )
