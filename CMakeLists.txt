cmake_minimum_required(VERSION 3.11 FATAL_ERROR)

project(gaisp
        LANGUAGES CXX CUDA
        DESCRIPTION "GPU accelerated interferometric SAR processing for Sentinel-1 TOPS data")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_CUDA_STANDARD 14)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

find_package(CUDA REQUIRED)
if (CUDA_FOUND)
    message(STATUS "Found CUDA ${CUDA_VERSION_STRING} at ${CUDA_TOOLKIT_ROOT_DIR}")
    INCLUDE_DIRECTORIES(${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
else (CUDA_FOUND)
    message(FATAL_ERROR "No CUDA toolchain found. Exiting.")
endif ()


list(APPEND TINYXML2_SOURCES
    tinyxml2/tinyxml2.cpp
    tinyxml2/tinyxml2.h
    )

list(APPEND SOURCES
    DataTypes.cpp
    DerampAndResample.cu
    EstCoh.cu
    EstESD.cu
    Func.cpp
    Geocoreg_cuda.cu
    Main.cpp
    )

add_executable(gaisp ${SOURCES} ${TINYXML2_SOURCES})
target_include_directories(gaisp
    PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/tinyxml2
    )
target_link_libraries(gaisp
    PRIVATE
    gdal
    )
