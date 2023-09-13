cmake_minimum_required(VERSION 3.16)

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
		../third_party/openFrameworks/libs/json/include)
target_compile_features(visualizer PUBLIC cxx_std_17)
target_link_libraries(visualizer lib-mapf)
