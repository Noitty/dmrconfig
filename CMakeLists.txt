cmake_minimum_required(VERSION 3.2)

# User-defined parameters (used as hints in find_package): -----------------------------------------
execute_process(COMMAND git describe --tags --abbrev=0 OUTPUT_VARIABLE DMRCONFIG_VERSION)
string(REPLACE "\n" "" DMRCONFIG_VERSION ${DMRCONFIG_VERSION})
message(STATUS "dmrconfing version: ${DMRCONFIG_VERSION}")

execute_process(COMMAND git rev-list HEAD --count OUTPUT_VARIABLE DMRCONFIG_REVISION)
string(REPLACE "\n" "" DMRCONFIG_REVISION ${DMRCONFIG_REVISION})
message(STATUS "dmrconfing head revision: ${DMRCONFIG_REVISION}")

set(PROJECT_CFLAGS -DVERSION='"${DMRCONFIG_VERSION}.${DMRCONFIG_REVISION}"')
project(dmrconfig VERSION "${DMRCONFIG_VERSION}.${DMRCONFIG_REVISION}")

####################################################################################################
# Build environment setup (for GNU G++): -----------------------------------------------------------
set(CMAKE_CXX_STANDARD 11)          # C++11 Standard.
set(CMAKE_CXX_STANDARD_REQUIRED ON) # Force required standard.
set(CMAKE_CXX_EXTENSIONS ON)        # Enable compiler extensions (gnu++11).
set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -g -Wall -Wextra -Wno-reorder -O2 -pthread -ludev ${PROJECT_CFLAGS}")
set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -g -Wall -Wextra -Wno-reorder -O2 -pthread -lpthread -ludev ${PROJECT_CFLAGS}")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

# External packages/libraries to include that are common for all sources: --------------------------
# libusb -- necessary for linux and mac
if(UNIX)
find_package(LIBUSB REQUIRED)
include_directories(SYSTEM ${LIBUSB_INCLUDE_DIRS})
link_libraries(${LIBUSB_LIBRARIES})
endif(UNIX)

# usb library for windows
if(WIN32)
find_package(LIBWINDOWS REQUIRED)
include_directories(SYSTEM ${LIBWINDOWS_INCLUDE_DIRS})
add_definitions(-DLIBWINDOWS_HAS_STDSTRING=1)
add_definitions(-DLIBWINDOWS_WRITE_DEFAULT_FLAGS=3)
endif(WIN32)

# Project's header locations: ----------------------------------------------------------------------
file(GLOB_RECURSE headers_list_src "src/*.h")
list(APPEND headers_list ${headers_list_src})

set(global_include_dirs "")
foreach(hfile ${headers_list})
    get_filename_component(hdir ${hfile} DIRECTORY)
    list(APPEND global_include_dirs ${hdir})
endforeach()
list(REMOVE_DUPLICATES global_include_dirs)

include_directories(${global_include_dirs})     # Adds all the folders with headers insdide.

# Prepare building units: --------------------------------------------------------------------------
add_subdirectory(src)
