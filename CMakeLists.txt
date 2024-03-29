CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

PROJECT(libnl-tiny C)
INCLUDE(GNUInstallDirs)

INCLUDE_DIRECTORIES(include)

ADD_DEFINITIONS(-Wall -Werror -Wextra -Wno-unused-parameter)
STRING(REPLACE "-DNDEBUG" "" CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE}")

SET(SOURCES
	attr.c
	cache.c
	cache_mngt.c
	error.c
	genl.c
	genl_ctrl.c
	genl_family.c
	genl_mngt.c
	handlers.c
	msg.c
	nl.c
	object.c
	socket.c
	unl.c
)

SET(SOVERSION 1 CACHE STRING "Override libnl-tiny library version")

ADD_LIBRARY(nl-tiny SHARED ${SOURCES})
SET_TARGET_PROPERTIES(nl-tiny PROPERTIES SOVERSION ${SOVERSION})

ADD_LIBRARY(nl-tiny-static STATIC ${SOURCES})
SET_TARGET_PROPERTIES(nl-tiny-static PROPERTIES OUTPUT_NAME nl-tiny)

CONFIGURE_FILE(
	"${CMAKE_CURRENT_SOURCE_DIR}/libnl-tiny.pc.in"
	"${CMAKE_CURRENT_BINARY_DIR}/libnl-tiny.pc"
	@ONLY
)

INSTALL(TARGETS nl-tiny nl-tiny-static LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
INSTALL(FILES ${CMAKE_BINARY_DIR}/libnl-tiny.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
INSTALL(
	DIRECTORY ${CMAKE_SOURCE_DIR}/include/
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/libnl-tiny
	FILES_MATCHING PATTERN "*.h"
)
