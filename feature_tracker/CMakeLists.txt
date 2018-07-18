cmake_minimum_required(VERSION 2.8.3)
project(feature_tracker)

# Compiler
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

# Dependencies

find_package(Eigen3)

find_package(catkin REQUIRED COMPONENTS
    roscpp std_msgs sensor_msgs camera_model)

set(cv_bridge_DIR "/home/nvidia/ws_opencv/deve/share/cv_bridge/cmake")
find_package(cv_bridge REQUIRED NO_CMAKE_PATH)

set(OpenCV_DIR "/usr/local")
find_package(OpenCV 3.4.1 REQUIRED NO_CMAKE_PATH)


catkin_package()


include_directories(include ${OpenCV_INCLUDE_DIRS} ${cv_bridge_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR}
${catkin_INCLUDE_DIRS})

#set(CMAKE_PREFIX_PATH /home/nvidia/opencv;${CMAKE_PREFIX_PATH})
#set(_include_dirs "include;/home/nvidia/opencv")

#########
# Build
#########
message(WARNING "OpenCV_VERSION: ${OpenCV_VERSION}")
message(WARNING "OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")
message(WARNING "OpenCV_LIBRARIES: ${OpenCV_LIBRARIES}")
message(WARNING "OpenCV_LIBS: ${OpenCV_LIBS}")
message(WARNING "CMAKE_PREFIX_PATH: ${CMAKE_PREFIX_PATH}")
message(WARNING "LD_LIBRARY_PATH: ${LD_LIBRARY_PATH}")
message(WARNING "_include_dirs: ${_include_dirs}")

add_library(feature_tracker_lib
	src/parameters.cpp
	src/feature_tracker.cpp
)
target_link_libraries(feature_tracker_lib 
	${OpenCV_LIBS} ${cv_bridge_LIBRARIES} ${catkin_LIBRARIES})

add_executable(feature_tracker
	src/feature_tracker_node.cpp
)
target_link_libraries(feature_tracker 
	feature_tracker_lib ${catkin_LIBRARIES}) #Note OpenCV_LIBS needs to be before cakin_libs
