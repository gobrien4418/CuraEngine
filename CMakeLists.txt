project(CuraEngine)

cmake_minimum_required(VERSION 2.8.12)

find_package(Arcus REQUIRED)

if(NOT ${CMAKE_VERSION} VERSION_LESS 3.1)
    set(CMAKE_CXX_STANDARD 11)
else()
    set(CMAKE_CXX_FLAGS "-std=c++11")
endif()

# Add warnings
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

if(NOT APPLE AND NOT WIN32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -static-libstdc++")
endif()

include_directories(${CMAKE_CURRENT_BINARY_DIR} libs)

add_library(clipper STATIC libs/clipper/clipper.cpp)

set(engine_SRCS
    src/bridge.cpp
    src/comb.cpp
    src/commandSocket.cpp
    src/FffGcodeWriter.cpp
    src/FffPolygonGenerator.cpp
    src/gcodeExport.cpp
    src/gcodePlanner.cpp
    src/infill.cpp
    src/inset.cpp
    src/layerPart.cpp
    src/main.cpp
    src/MergeInfillLines.cpp
    src/mesh.cpp
    src/MeshGroup.cpp
    src/multiVolumes.cpp
    src/pathOrderOptimizer.cpp
    src/PrimeTower.cpp
    src/Progress.cpp
    src/raft.cpp
    src/settingRegistry.cpp
    src/settings.cpp
    src/skin.cpp
    src/skirt.cpp
    src/slicer.cpp
    src/support.cpp
    src/timeEstimate.cpp
    src/wallOverlap.cpp
    src/Weaver.cpp
    src/Wireframe2gcode.cpp

    src/utils/gettime.cpp
    src/utils/logoutput.cpp
    src/utils/polygonUtils.cpp
    src/utils/polygon.cpp
)

protobuf_generate_cpp(engine_PB_SRCS engine_PB_HEADERS Cura.proto)

add_executable(CuraEngine ${engine_SRCS} ${engine_PB_SRCS})
target_link_libraries(CuraEngine clipper Arcus)

if (UNIX)
    target_link_libraries(CuraEngine pthread)
endif()

include(GNUInstallDirs)
install(TARGETS CuraEngine DESTINATION ${CMAKE_INSTALL_BINDIR})
include(CPackConfig.cmake)

