cmake_minimum_required(VERSION 3.16)

# set(OF_ROOT "${CMAKE_SOURCE_DIR}/third_party/openFrameworks")
# set(OF_INCLUDE "${OF_ROOT}/libs/openFrameworksCompiled/project/makefileCommon/compile.project.mk")
# include(../third_party/openFrameworks/libs/openFrameworksCompiled/project/makefileCommon/compile.project.mk)
include(../ReadVariables.cmake)
ReadVariables(../third_party/openFrameworks/libs/openFrameworksCompiled/project/makefileCommon/compile.project.mk)


find_package(PkgConfig REQUIRED)
pkg_check_modules(gtk3 REQUIRED IMPORTED_TARGET gtk+-3.0)
pkg_search_module(gstreamer REQUIRED IMPORTED_TARGET gstreamer-1.0>=1.4)
pkg_search_module(gstreamer-sdp REQUIRED IMPORTED_TARGET gstreamer-sdp-1.0>=1.4)
pkg_search_module(gstreamer-app REQUIRED IMPORTED_TARGET gstreamer-app-1.0>=1.4)
pkg_search_module(gstreamer-video REQUIRED IMPORTED_TARGET gstreamer-video-1.0>=1.4)


# Mod by Tim
add_executable(visualizer ./src/main.cpp ./src/ofApp.cpp)
include_directories(./include
		/usr/include/cairo
		../third_party/openFrameworks/libs/openFrameworks 
		../third_party/openFrameworks/libs/openFrameworks/utils
		../third_party/openFrameworks/libs/openFrameworks/math
		../third_party/openFrameworks/libs/openFrameworks/events
		../third_party/openFrameworks/libs/openFrameworks/types
		../third_party/openFrameworks/libs/openFrameworks/graphics
		../third_party/openFrameworks/libs/openFrameworks/communication
		../third_party/openFrameworks/libs/openFrameworks/gl
		../third_party/openFrameworks/libs/openFrameworks/app
		../third_party/openFrameworks/libs/openFrameworks/3d
		../third_party/openFrameworks/libs/openFrameworks/sound
		../third_party/openFrameworks/libs/openFrameworks/video
		../third_party/openFrameworks/libs/tess2/include
		../third_party/openFrameworks/libs/utf8/include
		../third_party/openFrameworks/libs/glm/include
		../third_party/openFrameworks/libs/json/include
		../third_party/openFrameworks/libs/kiss/include
		../third_party/openFrameworks/addons/ofxGui/src)
target_compile_features(visualizer PUBLIC cxx_std_17)
target_link_libraries(visualizer 
    PkgConfig::gtk3
    PkgConfig::gstreamer
    PkgConfig::gstreamer-sdp
    PkgConfig::gstreamer-app
    PkgConfig::gstreamer-video
    lib-mapf)
