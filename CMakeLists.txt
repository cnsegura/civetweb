#Copyright (c) Microsoft. All rights reserved.
#Licensed under the MIT license. See LICENSE file in the project root for full license information.

cmake_minimum_required(VERSION 2.8.11)

project(civet_web_client)


#making a nice global variable to know if we are on linux or not.
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set(LINUX TRUE)
endif()

#REENABLE AT A LATER DATE
#include (CTest)

#the following variables are project-wide and can be used with cmake-gui
option(skip_unittests "set skip_unittests to ON to skip unittests (default is OFF)[if possible, they are always build]" ON)

#these are the C source files
set(civet_web_sources
	./src/civetweb.c
)

#these are the C headers
set(civet_web_headers
	./include/civetweb.h
)

set(civet_web_static_sources
	${civet_web_sources}
)

set(civet_web_static_headers
	${civet_web_headers}
)

set(CIVET_WEB_INC_FOLDER ${CMAKE_CURRENT_LIST_DIR}/include CACHE INTERNAL "this is what needs to be included if using civet_web lib" FORCE)
include_directories(./include)
include_directories(${CIVET_WEB_INC_FOLDER})

#add support for websockets into the compiled civetweb
add_definitions(-DUSE_WEBSOCKET)

#this builds the civetweb dynamic library
add_library(civet_web 
	${civet_web_sources}  
	${civet_web_headers}
)

#this builds the civetweb static library
add_library(civet_web_static  ${civet_web_static_sources} ${civet_web_static_headers})
target_compile_definitions(civet_web_static PRIVATE BUILD_MODULE_TYPE_STATIC)

linkSharedUtil(civet_web)
linkSharedUtil(civet_web_static)




