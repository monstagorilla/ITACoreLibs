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

set( ITA_CORE_LIBS_BUILD_STATIC NOT ${BUILD_SHARED_LIBS} )

# Apps and tests and benchmarks
if( NOT DEFINED ITA_CORE_LIBS_WITH_APPS )
	set( ITA_CORE_LIBS_WITH_APPS ON CACHE BOOL "Build ITACoreLibs applications" )
endif()
if( NOT DEFINED ITA_CORE_LIBS_WITH_TESTS )
	set( ITA_CORE_LIBS_WITH_TESTS OFF CACHE BOOL "Build ITACoreLibs tests" )
endif( )
if( NOT DEFINED ITA_CORE_LIBS_WITH_BENCHMARKS )
	set( ITA_CORE_LIBS_WITH_BENCHMARKS OFF CACHE BOOL "Build ITACoreLibs benchmarks" )
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
add_subdirectory( ITADataSources )
add_subdirectory( ITAConvolution )
add_subdirectory( ITADSP )
add_subdirectory( ITACTC )
add_subdirectory( ITASampler )

# Selective license
if( CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR )
	if( ITA_SELECTIVE_LICENSE_DEPENDENCY_GPL )
	
		message( "The configuration you have selected requires copying under the terms of the GNU General Public License. This license includes a copyleft and may require your source code to be published under the same terms. Please respect the license agreements. Will generate the license for the distribution." )
		
		set( SELECTED_LICENSE "GNU General Public License" )
		set( GPL_DEPENDENCIES "FFTW3" )
		configure_file( COPYING.proto ${CMAKE_CURRENT_SOURCE_DIR}/COPYING @ONLY )
		install( FILES COPYING DESTINATION . )
		
	endif( )
else( )
	set( ITA_SELECTIVE_LICENSE_DEPENDENCY_GPL ${ITA_SELECTIVE_LICENSE_DEPENDENCY_GPL} PARENT_SCOPE )
endif( )


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
