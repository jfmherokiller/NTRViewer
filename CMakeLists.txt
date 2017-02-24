cmake_minimum_required(VERSION 3.6)
project(NTRViewer)

# Append our module directory to CMake
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)


find_package(SDL REQUIRED)
include_directories(${SDL_INCLUDE_DIR})
list(APPEND LIBS ${SDL_LIBRARY})

find_package(FFMPEG REQUIRED)
include_directories(SYSTEM ${FFMPEG_INCLUDE_DIR}) # use SYSTEM to suppress FFMPEG compile warnings
list(APPEND LIBS ${FFMPEG_LIBRARIES})

find_package(LibJpegTurbo REQUIRED)
include_directories(${LibJpegTurbo_INCLUDE_DIR})
list(APPEND LIBS ${LibJpegTurbo_LIBRARIES})


set(CMAKE_CXX_STANDARD 11)


set(SOURCE_FILES
        getopt.c
        getopt.h
        intdef.h
        NTRViewer.cpp
        targetver.h)
if(APPLE)
    add_executable(NTRViewer MACOSX_BUNDLE ${SOURCE_FILES})
    set_target_properties(NTRViewer PROPERTIES
            MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/resources/MacOSXBundleInfo.plist.in"
            )
else()
    add_executable(NTRViewer ${SOURCE_FILES})
endif(APPLE)

target_link_libraries(NTRViewer ${LIBS})

