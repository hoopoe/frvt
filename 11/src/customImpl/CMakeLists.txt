cmake_minimum_required(VERSION 2.8)

if(NOT FRVT_PROVIDER)
    set(FRVT_PROVIDER custom)
endif()

if(NOT FRVT_SEQUENCE)
    set(FRVT_SEQUENCE 001)
endif()

set(FRVT_11_NAME frvt_11_${FRVT_PROVIDER}_${FRVT_SEQUENCE})

project(${FRVT_11_NAME})

ADD_DEFINITIONS(-DLINUX)

find_package( OpenCV 4.1.2 REQUIRED core)

set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wno-write-strings")

include_directories (
	${CMAKE_CURRENT_SOURCE_DIR}/../include 
	${CMAKE_CURRENT_SOURCE_DIR}/../../../common/src/include
	${CMAKE_CURRENT_SOURCE_DIR}/../../external/msnet-mtcnn/include
)

# Configure built shared libraries in top-level lib directory
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../../lib)

AUX_SOURCE_DIRECTORY(. DIR_SRCS)

# Build the shared libraries
add_library (${FRVT_11_NAME} SHARED ${DIR_SRCS})

target_link_libraries (${FRVT_11_NAME} ${CMAKE_CURRENT_SOURCE_DIR}/../../lib/libmtcnn.so ${OpenCV_LIBS})


