cmake_minimum_required(VERSION 3.1)
project(CMakeDemo)

function(dump_cmake_variables)
    get_cmake_property(_variableNames VARIABLES)
    list (SORT _variableNames)
    foreach (_variableName ${_variableNames})
        if (ARGV0)
            unset(MATCHED)
            string(REGEX MATCH ${ARGV0} MATCHED ${_variableName})
            if (NOT MATCHED)
                continue()
            endif()
        endif()
        message(STATUS "${_variableName}=${${_variableName}}")
    endforeach()
endfunction()

function(print_target_properties tgt)
    if(NOT TARGET ${tgt})
      message("There is no target named '${tgt}'")
      return()
    endif()

    foreach (prop ${CMAKE_PROPERTY_LIST})
        string(REPLACE "<CONFIG>" "${CMAKE_BUILD_TYPE}" prop ${prop})
        get_target_property(propval ${tgt} ${prop})
        if (propval)
            get_target_property(propval ${tgt} ${prop})
            message ("${tgt} ${prop} = ${propval}")
        endif()
    endforeach(prop)
endfunction(print_target_properties)

include(FetchContent)
include (ExternalProject)
set(TheNew 1)

SET(CMAKE_FIND_LIBRARY_SUFFIXES .lib .a)

if(${TheNew})
	#set(CMAKE_BUILD_TYPE "Release" CACHE STRING "type of build" FORCE)
	set(FETCHCONTENT_QUIET OFF)

	
ExternalProject_Add(SDL2     
    URL  https://www.libsdl.org/release/SDL2-2.0.12.zip
	INSTALL_DIR    ${CMAKE_CURRENT_BINARY_DIR}/ext/
	INSTALL_COMMAND ""
	BUILD_COMMAND     ${CMAKE_COMMAND} --build <BINARY_DIR> --config Debug
 )
	#ExternalProject_Get_Property(SDL2 install_dir)
	#add_dependencies(CMakeDemo SDL2)
	
	ExternalProject_Get_Property(SDL2 SOURCE_DIR)
	ExternalProject_Get_Property(SDL2 binary_dir)
	
	include_directories("${SOURCE_DIR}/include")
	link_directories("${binary_dir}/Debug")
	
	#set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "type of build" FORCE)
	# Define executable target
	add_executable(CMakeDemo main.cpp)

	target_link_libraries(CMakeDemo SDL2-staticd winmm.lib imm32.lib version.lib Setupapi.lib)
else()

	set(CMAKE_SDL_PATH "${CMAKE_SOURCE_DIR}/extern/SDL2")
	set(CMAKE_SDL_INCLUDE_PATH "${CMAKE_SDL_PATH}/include")
	set(CMAKE_SDL_LIB_PATH "${CMAKE_SDL_PATH}/lib/x64-static")
  
	# Define executable target
	include_directories(${CMAKE_SDL_INCLUDE_PATH})
	add_executable(CMakeDemo main.cpp)

	target_link_libraries(CMakeDemo ${CMAKE_SDL_LIB_PATH}/SDL2-static.lib ${CMAKE_SDL_LIB_PATH}/SDL2main.lib winmm.lib imm32.lib version.lib Setupapi.lib)

endif()

message("******************************")
dump_cmake_variables()
message("#################")
print_target_properties(SDL2)


