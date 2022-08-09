cmake_minimum_required (VERSION 3.20 FATAL_ERROR)

project (
	ITACoreLibs
	VERSION 2022.0
	LANGUAGES CXX C
)

# Set the install root folder if not specified by the user
if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
	set (
		CMAKE_INSTALL_PREFIX
		${CMAKE_CURRENT_BINARY_DIR}/dist
		CACHE PATH "Install root directory" FORCE
	)
	set (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT OFF)
endif (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)

if (NOT DEFINED CMAKE_RUNTIME_OUTPUT_DIRECTORY AND WIN32)
	set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/bin)
	set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/lib)
	set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/lib)
endif ()

if (CMAKE_GENERATOR MATCHES "Visual Studio")
	set_property (GLOBAL PROPERTY USE_FOLDERS ON)
endif ()

# ---Options---

# Shared libs switch
option (BUILD_SHARED_LIBS "Build shared librarys" ON)
set (ITA_CORE_LIBS_BUILD_STATIC NOT ${BUILD_SHARED_LIBS})

option (ITA_VISTA_BUILD_STATIC "Build against static ViSTA libraries" OFF)
option (ITA_CORE_LIBS_WITH_APPS "Build ITACoreLibs applications" OFF)
option (ITA_CORE_LIBS_WITH_TESTS "Build ITACoreLibs tests" OFF)
option (ITA_CORE_LIBS_WITH_BENCHMARKS "Build ITACoreLibs benchmarks" OFF)

# Because submodules (and RAVEN) require some legacy code, overwrite default settings of ITABase
option (ITA_CORE_LIBS_LEGACY_SUPPORT "Enable legacy code support for some submodules (and RAVEN)" ON)

# ---End: Options---

# ---Overwrite the settings in the subprojects---
# cmake-format: off
set (ITA_BASE_WITH_OLD_ATOMICS ${ITA_CORE_LIBS_LEGACY_SUPPORT} CACHE BOOL "Build with old atomic code for non-C++11 compatible compilers")
set (ITA_BASE_WITH_REGULAR_EXPRESSIONS ${ITA_CORE_LIBS_LEGACY_SUPPORT} CACHE BOOL "Build with old regular expressions code for string manipulation")
set (ITA_BASE_WITH_CONFIG_OLD_IMPL ${ITA_CORE_LIBS_LEGACY_SUPPORT} CACHE BOOL "Build with old INI file configuration implementation (uses legacy regular expressions)")
set (ITA_BASE_WITH_OLD_RAVEN_OPS ${ITA_CORE_LIBS_LEGACY_SUPPORT} CACHE BOOL "Build with old RavenOps helper functions implementation (legacy code for RAVEN compatibility)")

# Tests
set (ITA_BASE_WITH_TESTS ${ITA_CORE_LIBS_WITH_TESTS} CACHE BOOL "Build the tests for the library" FORCE)
set (ITA_CONVOLUTION_WITH_TESTS ${ITA_CORE_LIBS_WITH_TESTS} CACHE BOOL "Build the tests for the library" FORCE)
set (ITA_CTC_WITH_TESTS ${ITA_CORE_LIBS_WITH_TESTS} CACHE BOOL "Build the tests for the library" FORCE)
set (ITA_DATA_SOURCES_WITH_TESTS ${ITA_CORE_LIBS_WITH_TESTS} CACHE BOOL "Build the tests for the library" FORCE)
set (ITA_DSP_WITH_TESTS ${ITA_CORE_LIBS_WITH_TESTS} CACHE BOOL "Build the tests for the library" FORCE)
set (ITA_FFT_WITH_TESTS ${ITA_CORE_LIBS_WITH_TESTS} CACHE BOOL "Build the tests for the library" FORCE)
set (ITA_SAMPLER_WITH_TESTS ${ITA_CORE_LIBS_WITH_TESTS} CACHE BOOL "Build the tests for the library" FORCE)
mark_as_advanced (ITA_BASE_WITH_TESTS)
mark_as_advanced (ITA_CONVOLUTION_WITH_TESTS)
mark_as_advanced (ITA_CTC_WITH_TESTS)
mark_as_advanced (ITA_DATA_SOURCES_WITH_TESTS)
mark_as_advanced (ITA_DSP_WITH_TESTS)
mark_as_advanced (ITA_FFT_WITH_TESTS)
mark_as_advanced (ITA_SAMPLER_WITH_TESTS)

# Apps
set (ITA_CONVOLUTION_WITH_APPS ${ITA_CORE_LIBS_WITH_APPS} CACHE BOOL "Build the applications of the library" FORCE)
set (ITA_DATA_SOURCES_WITH_APPS ${ITA_CORE_LIBS_WITH_APPS} CACHE BOOL "Build the applications of the library" FORCE)
mark_as_advanced (ITA_CONVOLUTION_WITH_APPS)
mark_as_advanced (ITA_DATA_SOURCES_WITH_APPS)

# Benchmarks
set (ITA_CONVOLUTION_WITH_BENCHMARKS ${ITA_CORE_LIBS_WITH_BENCHMARKS} CACHE BOOL "Build the benchmarks of the library" FORCE)
set (ITA_DSP_WITH_BENCHMARKS ${ITA_CORE_LIBS_WITH_BENCHMARKS} CACHE BOOL "Build the benchmarks of the library" FORCE)
mark_as_advanced (ITA_CONVOLUTION_WITH_BENCHMARKS)
mark_as_advanced (ITA_DSP_WITH_BENCHMARKS)
# cmake-format: on

# ---END: Overwrite the settings in the subprojects---

# Compiler
if (MSVC)
	add_compile_options (/wd4251 /W4 -D_CRT_SECURE_NO_WARNINGS)
endif ()

# ITACoreLibs submodules
add_subdirectory (ITABase)
add_subdirectory (ITAFFT)
add_subdirectory (ITADataSources)
add_subdirectory (ITAConvolution)
add_subdirectory (ITADSP)
add_subdirectory (ITACTC)
add_subdirectory (ITASampler)

# Add all-build target (can also be used to check whether ITACoreLibs are already included)
add_custom_target (
	${PROJECT_NAME}
	DEPENDS ITABase::ITABase
			ITAFFT::ITAFFT
			ITADataSources::ITADataSources
			ITAConvolution::ITAConvolution
			ITADSP::ITADSP
			ITACTC::ITACTC
			ITASampler::ITASampler
	COMMAND ${CMAKE_COMMAND} -E echo "----------------------------------------------"
	COMMAND ${CMAKE_COMMAND} -E echo "Finished building all ITACorelibs subprojects."
	COMMAND ${CMAKE_COMMAND} -E echo "----------------------------------------------"
)
set_property (TARGET ${PROJECT_NAME} PROPERTY FOLDER "BuildProjectCollections")

# Install - Selective license
if (${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})
	if (ITA_SELECTIVE_LICENSE_DEPENDENCY_GPL)

		message (
			"The configuration you have selected requires copying under the terms of the GNU General Public License. This license includes a copyleft and may require your source code to be published under the same terms. Please respect the license agreements. Will generate the license for the distribution."
		)

		set (SELECTED_LICENSE "GNU General Public License")
		set (GPL_DEPENDENCIES "FFTW3")
		configure_file (COPYING.proto ${CMAKE_CURRENT_BINARY_DIR}/COPYING @ONLY)
		install (
			FILES ${CMAKE_CURRENT_BINARY_DIR}/COPYING
			DESTINATION .
			COMPONENT ${PROJECT_NAME}
		)

	endif ()
else ()
	set (
		ITA_SELECTIVE_LICENSE_DEPENDENCY_GPL
		${ITA_SELECTIVE_LICENSE_DEPENDENCY_GPL}
		PARENT_SCOPE
	)
endif ()
