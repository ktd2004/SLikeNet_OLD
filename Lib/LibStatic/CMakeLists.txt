#
# This file was taken from RakNet 4.082.
# Please see licenses/RakNet license.txt for the underlying license and related copyright.
#
#
# Modified work: Copyright (c) 2016-2018, SLikeSoft UG (haftungsbeschrankt)
#
# This source code was modified by SLikeSoft. Modifications are licensed under the MIT-style
# license found in the license.txt file in the root directory of this source tree.
#

cmake_minimum_required(VERSION 2.6)
project(RakNetLibStatic)

FILE(GLOB ALL_HEADER_SRCS ${RakNet_SOURCE_DIR}/Source/include/slikenet/*.h)
FILE(GLOB ALL_COMPATIBILITY_HEADER_SRC ${RakNet_SOURCE_DIR}/Source/*.h)
FILE(GLOB ALL_COMPATIBILITY_HEADER_SRC_2 ${RakNet_SOURCE_DIR}/Source/slikenet/*.h)
FILE(GLOB ALL_CPP_SRCS ${RakNet_SOURCE_DIR}/Source/src/*.cpp)

include_directories( ${RAKNET_INTERNAL_INCLUDE_DIRS} ) 

add_library(RakNetLibStatic STATIC ${ALL_CPP_SRCS} ${ALL_HEADER_SRCS})

IF(WIN32 AND NOT UNIX)
	SET( CMAKE_CXX_FLAGS "/D WIN32 /D _RAKNET_LIB /D _CRT_NONSTDC_NO_DEPRECATE /D _CRT_SECURE_NO_DEPRECATE /GS- /GR- ")
ENDIF(WIN32 AND NOT UNIX)

target_link_libraries (RakNetLibStatic ${RAKNET_LIBRARY_LIBS})
IF(WIN32 AND NOT UNIX)
	IF(NOT ${CMAKE_GENERATOR} STREQUAL "MSYS Makefiles")
        IF( MSVC_VERSION EQUAL 1500 )
            set_target_properties(RakNetLibStatic PROPERTIES STATIC_LIBRARY_FLAGS "/NODEFAULTLIB:&quot;LIBCD.lib LIBCMTD.lib MSVCRT.lib&quot;" )
        ELSE()
            set_target_properties(RakNetLibStatic PROPERTIES STATIC_LIBRARY_FLAGS "/NODEFAULTLIB:\"LIBCD.lib LIBCMTD.lib MSVCRT.lib\"" )
        ENDIF()
	ENDIF(NOT ${CMAKE_GENERATOR} STREQUAL "MSYS Makefiles")
ELSE(WIN32 AND NOT UNIX)
	INSTALL(TARGETS RakNetLibStatic DESTINATION lib)
	INSTALL(FILES ${ALL_COMPATIBILITY_HEADER_SRC} DESTINATION include/raknet)
	INSTALL(FILES ${ALL_COMPATIBILITY_HEADER_SRC_2} DESTINATION include/raknet/slikenet)
	INSTALL(FILES ${ALL_HEADER_SRCS} DESTINATION include/raknet/include/slikenet)
ENDIF(WIN32 AND NOT UNIX)
