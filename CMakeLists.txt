cmake_minimum_required(VERSION 2.8)
include(ExternalProject)
project(ExternalDemo)

set(EP_PREFIX "${CMAKE_BINARY_DIR}/3rdparty")

ExternalProject_Add(zlib
	URL https://github.com/madler/zlib/archive/v1.2.8.tar.gz
	CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${EP_PREFIX}
)

ExternalProject_Add(libpng
	URL https://github.com/glennrp/libpng/archive/v1.6.16.tar.gz
	DEPENDS zlib
	CMAKE_ARGS
	  -DCMAKE_INSTALL_PREFIX:PATH=${EP_PREFIX}
	  -DCMAKE_PREFIX_PATH:PATH=${EP_PREFIX}
)

# Add a "local" external project.
# As odd as it sounds, it enables the project to nicely depends on externals
# without having to fiddle with (include|link)_directories.
ExternalProject_Add(${PROJECT_NAME}
	SOURCE_DIR ${CMAKE_SOURCE_DIR}/demo
	PREFIX ${CMAKE_BINARY_DIR}
	DEPENDS zlib libpng
	BINARY_DIR ${CMAKE_BINARY_DIR}/bin
	CMAKE_ARGS
	  -DCMAKE_PREFIX_PATH:PATH=${EP_PREFIX}
	INSTALL_COMMAND ""
)

# Inspired from Kitware slicer SuperBuild
# https://github.com/Slicer/Slicer/blob/master/SuperBuild.cmake
# It will *not* recompile everything everytime.
# It just forces CMake to recheck for file changes.
ExternalProject_Add_Step(${PROJECT_NAME} force-build
	COMMAND ${CMAKE_COMMAND} -E remove
	  ${CMAKE_CURRENT_BINARY_DIR}/src/${PROJECT_NAME}-stamp/${PROJECT_NAME}-build
	COMMENT "Forcing build step for '${PROJECT_NAME}'"
	DEPENDEES build
	ALWAYS 1
)
