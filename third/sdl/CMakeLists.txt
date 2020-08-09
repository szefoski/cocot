cmake_minimum_required(VERSION 3.1)
project(SDL_DOWNLOAD)

include (ExternalProject)
include(ProcessorCount)
ProcessorCount(number_of_cores)

message("************** PREAPRE SDL2 ${N} *****************")

ExternalProject_Add(SDL2-download
    URL  https://www.libsdl.org/release/SDL2-2.0.12.zip
	INSTALL_DIR    ${CMAKE_CURRENT_BINARY_DIR}/ext/
	INSTALL_COMMAND ""
	CMAKE_ARGS -DCMAKE_BUILD_TYPE=Debug
	BUILD_COMMAND     ${CMAKE_COMMAND} --build <BINARY_DIR> --config Debug -j ${number_of_cores}
	LOG_CONFIGURE OFF
 )