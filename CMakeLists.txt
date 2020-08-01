cmake_minimum_required(VERSION 3.1)
project(CMakeDemo)

set(CMAKE_SDL_PATH "${CMAKE_SOURCE_DIR}/extern/SDL2")
set(CMAKE_SDL_INCLUDE_PATH "${CMAKE_SDL_PATH}/include")
set(CMAKE_SDL_LIB_PATH "${CMAKE_SDL_PATH}/lib/x64-static")
message("sdl include path: ${CMAKE_SDL_INCLUDE_PATH}")

# Define executable target
include_directories(${CMAKE_SDL_INCLUDE_PATH})
add_executable(CMakeDemo main.cpp)

target_link_libraries(CMakeDemo ${CMAKE_SDL_LIB_PATH}/SDL2-static.lib ${CMAKE_SDL_LIB_PATH}/SDL2main.lib winmm.lib imm32.lib version.lib Setupapi.lib)