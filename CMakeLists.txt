# (c) 2025, Cary Clark cclark2@gmail.com

cmake_minimum_required (VERSION 3.16)
project (pov0debugger VERSION 0.1)

set (CMAKE_CXX_STANDARD 17)
set (CMAKE_CXX_STANDARD_REQUIRED ON)
set (CMAKE_CXX_EXTENSIONS OFF)

# set the output directory for built objects.
# This makes sure that the dynamic library goes into the build directory automatically.
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/$<CONFIGURATION>")
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/$<CONFIGURATION>")

find_package (SDL3 REQUIRED)

find_package (SDL3_ttf REQUIRED)

include_directories(
	../SDL3_ttf-3.2.2/include
	../SDL3-3.2.24/include
	../pentrek_raster
	../pathopsv0/debugger
	../pathopsv0/debug
	../pathopsv0/public
	../pathopsv0/src
	../pathopsv0/tests
)

add_executable (v0debugger 
	../pathopsv0/debugger/main.cpp 
	../pathopsv0/debugger/DebuggerDraw.cpp    
	../pathopsv0/debugger/DebuggerPoly.cpp    
	../pathopsv0/debugger/DebuggerState.cpp    
	../pathopsv0/debugger/DebuggerTypes.cpp    
	../pathopsv0/debugger/DebuggerWindow.cpp    
	../pathopsv0/debugger/HelpWindow.cpp    
	../pathopsv0/debugger/PictureWindow.cpp    
	../pathopsv0/debugger/TextWindow.cpp    
 	../pathopsv0/debug/OpDebug.cpp
	../pathopsv0/src/OpContext.cpp
	../pathopsv0/src/OpContour.cpp
	../pathopsv0/src/OpCubic.cpp
	../pathopsv0/src/OpCurve.cpp
	../pathopsv0/src/OpCurveCurve.cpp
	../pathopsv0/src/OpEdge.cpp
	../pathopsv0/src/OpIntersection.cpp
	../pathopsv0/src/OpJoiner.cpp
	../pathopsv0/src/OpMath.cpp
	../pathopsv0/src/OpSegment.cpp
	../pathopsv0/src/OpSegments.cpp
    ../pathopsv0/src/OpWinder.cpp
    ../pathopsv0/src/OpWinding.cpp
    ../pathopsv0/engine/PathOps.cpp
	../pathopsv0/debug/OpDebugColor.cpp
	../pathopsv0/debug/OpDebugDump.cpp
	../pathopsv0/debug/OpDebugTags.cpp
)

# Link to the actual SDL3 library.
target_link_libraries (v0debugger PRIVATE 
    /Users/cary/SDL_ttf/lib/libSDL3_ttf.0.2.2.dylib 
	/Users/cary/SDL/SDL3.framework/Versions/A/SDL3 
	/Users/cary/pentrek_raster/build/libpentrek_raster.a
)

target_compile_definitions (v0debugger PUBLIC OP_DEBUGGER NOMINMAX _CRT_SECURE_NO_WARNINGS)

if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options (v0debugger PUBLIC)
else()
    target_compile_options (v0debugger PUBLIC -g -Wall -Wpedantic -Wno-unused-parameter -Wno-invalid-offsetof)
endif()
