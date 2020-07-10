#-------------------------------------------------------------------------------------------
# Copyright (C) Corsac Engine All rights reserved.
#-------------------------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.17.3)

project(Corsac2D VERSION 0.1.0)
include_directories (${CMAKE_SOURCE_DIR}/include)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

#-------------------------------------------------------------------------------------------
# EASTL
#-------------------------------------------------------------------------------------------
set(EASTL_ROOT_DIR ${CMAKE_SOURCE_DIR}/lib/EASTL)
include_directories (${EASTL_ROOT_DIR}/include)
include_directories (${EASTL_ROOT_DIR}/test/packages/EAAssert/include)
include_directories (${EASTL_ROOT_DIR}/test/packages/EABase/include/Common)
include_directories (${EASTL_ROOT_DIR}/test/packages/EAMain/include)
include_directories (${EASTL_ROOT_DIR}/test/packages/EAStdC/include)
include_directories (${EASTL_ROOT_DIR}/test/packages/EATest/include)
include_directories (${EASTL_ROOT_DIR}/test/packages/EAThread/include)

if (WIN32)
    set(EASTL_LIBRARY debug ${EASTL_ROOT_DIR}/build/Debug/EASTL.lib optimized ${EASTL_ROOT_DIR}/build/Release/EASTL.lib)
    message (STATUS "Это Windows")
elseif (UNIX)
    set(EASTL_LIBRARY debug ${EASTL_ROOT_DIR}/build/libEASTL.a optimized ${EASTL_ROOT_DIR}/build/libEASTL.a)
    message (STATUS "Это Unix")
else ()
	message (FATAL_ERROR "Неизвестная система")
endif ()

add_custom_target(NatVis SOURCES ${EASTL_ROOT_DIR}/doc/EASTL.natvis)

message (STATUS "EASTL -> is include!")

#-------------------------------------------------------------------------------------------
# SDL2
#-------------------------------------------------------------------------------------------
find_package(SDL2 REQUIRED)
find_package(SDL2_image REQUIRED)
find_package(SDL2_mixer REQUIRED)
include_directories(
	${SDL2_INCLUDE_DIRS}
	${SDL2_IMAGE_INCLUDE_DIRS}
    ${SDL2_MIXER_INCLUDE_DIRS}
)

message (STATUS "SDL2 -> is include!")
message (STATUS "SDL2_image -> is include!")
message (STATUS "SDL2_mixer -> is include!")
#-------------------------------------------------------------------------------------------
# BIN
#-------------------------------------------------------------------------------------------
file(GLOB TARGET_SRC "./src/*.cpp" "./include/*.h")
add_executable(${PROJECT_NAME} ${TARGET_SRC})
set_target_properties( ${PROJECT_NAME}
		PROPERTIES
		CXX_STANDARD 17
		CXX_STANDARD_REQUIRED ON
)
#-------------------------------------------------------------------------------------------
# Dependencies
#-------------------------------------------------------------------------------------------
target_link_libraries(${PROJECT_NAME}
	${SDL2_LIBRARIES}
	${SDL2_IMAGE_LIBRARIES}
    ${SDL2_MIXER_LIBRARIES}
    ${EASTL_LIBRARY}
    -lSDL2main -lSDL2 -lSDL2_image -lSDL2_mixer
)

#-------------------------------------------------------------------------------------------
# CPACK
#-------------------------------------------------------------------------------------------
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
