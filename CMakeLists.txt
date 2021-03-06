cmake_minimum_required(VERSION 3.0)

project(sqlite3 VERSION 3.31.1)

option(BUILD_SHARED "Build shared library" ON)
option(BUILD_STATIC "Build static archive" ON)

find_package(Threads REQUIRED)

macro(sqlite3_add_dependencies library)
	target_include_directories("${library}" INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}")
	target_link_libraries("${library}" PUBLIC ${CMAKE_DL_LIBS})
	target_link_libraries("${library}" PUBLIC Threads::Threads)
endmacro()

if(BUILD_SHARED)
	add_library(sqlite3 SHARED sqlite3.c sqlite3.h)
	sqlite3_add_dependencies(sqlite3)
endif()

if(BUILD_STATIC)
	add_library(sqlite3-static STATIC sqlite3.c sqlite3.h)
	sqlite3_add_dependencies(sqlite3-static)
endif()

