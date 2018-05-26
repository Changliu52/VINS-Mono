cmake_minimum_required(VERSION 2.8.3)
project(feature_tracker)

# Compiler
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

# Dependencies
include_directories(include)

find_package(OpenCV 3 REQUIRED
        PATHS /usr/local/opencv_3.3.0
        NO_DEFAULT_PATH
	CONFIG)
include_directories(${OpenCV_INCLUDE_DIRS})

find_package(Eigen3)
include_directories(${EIGEN3_INCLUDE_DIR})

set(cv_bridge_DIR /usr/local/share/cv_bridge/cmake)
find_package(catkin REQUIRED COMPONENTS
    roscpp std_msgs sensor_msgs cv_bridge camera_model)
include_directories(${catkin_INCLUDE_DIRS})


# Build
catkin_package(INCLUDE_DIRS include
		CATKIN_DEPENDS std_msgs sensor_msgs camera_model cv_bridge
		DEPENDS OpenCV
		#LIBRARIES ${PROJECT_NAME}
)

add_executable(feature_tracker
    src/feature_tracker_node.cpp
    src/parameters.cpp
    src/feature_tracker.cpp
)

target_link_libraries(feature_tracker ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
