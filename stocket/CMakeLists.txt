cmake_minimum_required(VERSION 3.7)

project(socket.exe VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#=================== Find Library ================#
find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS})


#=================== INCLUSION OF Project Files ================#
set(SOURCE_DIR "${CMAKE_SOURCE_DIR}/src")

file(GLOB_RECURSE SOURCES
    "${SOURCE_DIR}/*.h"
    "${SOURCE_DIR}/*.cpp"
)

add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})

#===================== LINKING LIBRARIES =======================#
set_target_properties(${CMAKE_PROJECT_NAME} PROPERTIES
                        RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR})

target_link_libraries(${CMAKE_PROJECT_NAME} 
                      ${SDL2_LIBRARY}
                      )