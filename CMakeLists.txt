CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

PROJECT(libnl-tiny C)
INCLUDE(GNUInstallDirs)

INCLUDE_DIRECTORIES(include)

ADD_DEFINITIONS(-Wall -Werror -Wextra -Wno-unused-parameter)
STRING(REPLACE "-DNDEBUG" "" CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE}")

ADD_LIBRARY(nl-tiny SHARED
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

CONFIGURE_FILE(
	"${CMAKE_CURRENT_SOURCE_DIR}/libnl-tiny.pc.in"
	"${CMAKE_CURRENT_BINARY_DIR}/libnl-tiny.pc"
	@ONLY
)

INSTALL(TARGETS nl-tiny LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
INSTALL(
	DIRECTORY ${CMAKE_SOURCE_DIR}/include/
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/libnl-tiny
	FILES_MATCHING PATTERN "*.h"
)
