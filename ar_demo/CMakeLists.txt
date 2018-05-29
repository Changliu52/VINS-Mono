cmake_minimum_required(VERSION 2.8.3)
project(ar_demo)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11 -DEIGEN_DONT_PARALLELIZE")
#-DEIGEN_USE_MKL_ALL")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  image_transport
  sensor_msgs
  message_filters
  camera_model
)

set(cv_bridge_DIR "/home/nvidia/ws_opencv/deve/share/cv_bridge/cmake")
find_package(cv_bridge REQUIRED NO_CMAKE_PATH)

set(OpenCV_DIR "/home/nvidia/opencv/build")
find_package(OpenCV 3 REQUIRED NO_CMAKE_PATH)

catkin_package(
        LIBRARIES ${PROJECT_NAME}
)

find_package(Eigen3)

include_directories(
	${OpenCV_INCLUDE_DIRS}
	${cv_bridge_INCLUDE_DIRS}
	${EIGEN3_INCLUDE_DIR}
	${catkin_INCLUDE_DIRS}
)

add_executable(ar_demo_node src/ar_demo_node.cpp)

 target_link_libraries(ar_demo_node
	${OpenCV_LIBS}
	${cv_bridge_LIBRARIES}
	${catkin_LIBRARIES}
 )


