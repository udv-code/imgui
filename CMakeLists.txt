cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

# Global configs
set(CMAKE_DEBUG_POSTFIX "-debug")

# CMake options
option(IMGUI_BUILD_EXAMPLES "Build examples" OFF)

# Project config
set(IMGUI_PROJECT_NAME "imgui")
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
		LANGUAGES C CXX
)

# Vendor

# Target
add_library(
		${IMGUI_TARGET_NAME}
		STATIC
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

target_include_directories(
		${IMGUI_TARGET_NAME}

		PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
		$<INSTALL_INTERFACE:include>

		INTERFACE

		PRIVATE
)

if (IMGUI_BUILD_EXAMPLES)
	add_subdirectory(examples)
endif ()