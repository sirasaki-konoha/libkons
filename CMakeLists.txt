cmake_minimum_required(VERSION 3.10)
project(libkons)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Option to specify libkcore location
set(LIBKCORE_DIR "" CACHE PATH "Path to libkcore build directory")

# Build order
add_subdirectory(libkcore)

# Mark that kcore targets are available for libkonio
set(KCORE_TARGETS_AVAILABLE TRUE CACHE INTERNAL "")

if(LIBKCORE_DIR)
    message(STATUS "Using libkcore from: ${LIBKCORE_DIR}")
    set(LIBKCORE_INCLUDE_DIR "${LIBKCORE_DIR}/src")
    set(LIBKCORE_LIB "libkcore")
    link_directories("${LIBKCORE_DIR}/build")
elseif(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/libkcore/src/kcore.h")
    message(STATUS "Auto-detected local libkcore")
    set(LIBKCORE_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/libkcore/src")
    set(LIBKCORE_LIB "libkcore")
else()
    message(STATUS "Using system libkcore")
    set(LIBKCORE_INCLUDE_DIR "")
    set(LIBKCORE_LIB "kcore")
endif()

add_subdirectory(libkonio)

# Install root header file
install(FILES kons.h 
    DESTINATION include
)
