cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

# Global configs
set(CMAKE_DEBUG_POSTFIX "-debug")
set(CMAKE_CXX_STANDARD 17)

# CMake options
option(IMGUI_BUILD_EXAMPLES "Build examples" OFF)
option(BUILD_SHARED_LIBS "Build shared." ON)

# Project config
set(IMGUI_PROJECT_NAME "imgui")
set(IMGUI_VERSION_MAJOR 1)
set(IMGUI_VERSION_MINOR 0)
set(IMGUI_VERSION_HOTFIX 0)
set(IMGUI_VERSION "${IMGUI_VERSION_MAJOR}.${IMGUI_VERSION_MINOR}.${IMGUI_VERSION_HOTFIX}")
set(IMGUI_NAME_VERSION "${IMGUI_PROJECT_NAME} v${IMGUI_VERSION}")
# Target Config
set(IMGUI_TARGET_NAME "${IMGUI_PROJECT_NAME}")

if (WIN32)
	set(CMAKE_USE_RELATIVE_PATHS ON)
endif ()

# Modules
list(INSERT CMAKE_MODULE_PATH 0 "${CMAKE_CURRENT_LIST_DIR}/cmake")

# Project setup
project(
		${IMGUI_PROJECT_NAME}
		VERSION ${IMGUI_VERSION}
		LANGUAGES C CXX
)

# Vendor

# Target
add_library(
		${IMGUI_TARGET_NAME}

		imgui.h
		imgui.cpp

		imconfig.h

		imgui_internal.h

		imgui_draw.cpp
		imgui_demo.cpp
		imgui_widgets.cpp

		imstb_rectpack.h
		imstb_textedit.h
		imstb_truetype.h
)

add_library(
		${IMGUI_PROJECT_NAME}::${IMGUI_TARGET_NAME}
		ALIAS
		${IMGUI_TARGET_NAME}
)

set_target_properties(
		${IMGUI_TARGET_NAME}
		PROPERTIES

		RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
		ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
		LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
)

target_include_directories(
		${IMGUI_TARGET_NAME}

		PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
		$<INSTALL_INTERFACE:include>

		INTERFACE

		PRIVATE
)

target_compile_definitions(
		${IMGUI_TARGET_NAME}

		PUBLIC
		${DEFAULT_COMPILE_DEFINITIONS}

		INTERFACE

		PRIVATE
)

target_compile_options(
		${IMGUI_TARGET_NAME}

		PUBLIC
		${DEFAULT_COMPILE_OPTIONS}

		INTERFACE

		PRIVATE
)

target_link_libraries(
		${IMGUI_TARGET_NAME}

		PUBLIC
		${DEFAULT_LINKER_OPTIONS}

		INTERFACE

		PRIVATE
)

if (IMGUI_BUILD_EXAMPLES)
	add_subdirectory(examples)
endif ()