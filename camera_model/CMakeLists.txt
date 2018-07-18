cmake_minimum_required(VERSION 2.8.3)
project(camera_model)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -fPIC")

find_package(catkin REQUIRED COMPONENTS
    roscpp
    std_msgs
    )


find_package(Boost REQUIRED COMPONENTS filesystem program_options system)
find_package(Ceres REQUIRED)

set(OpenCV_DIR "/usr/local")
find_package(OpenCV 3.4.1 REQUIRED NO_CMAKE_PATH)


catkin_package(
	INCLUDE_DIRS include
	LIBRARIES camera_model
	CATKIN_DEPENDS roscpp std_msgs
)


include_directories(
	include
	${OpenCV_INCLUDE_DIRS}
	${Boost_INCLUDE_DIRS}
	${CERES_INCLUDE_DIRS}
	${catkin_INCLUDE_DIRS}
)


##########
# BUILD
##########
message(WARNING "OpenCV_VERSION: ${OpenCV_VERSION}")
message(WARNING "OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")
message(WARNING "OpenCV_LIBRARIES: ${OpenCV_LIBRARIES}")
message(WARNING "OpenCV_LIBS: ${OpenCV_LIBS}")
message(WARNING "CMAKE_PREFIX_PATH: ${CMAKE_PREFIX_PATH}")
message(WARNING "LD_LIBRARY_PATH: ${LD_LIBRARY_PATH}")
message(WARNING "_include_dirs: ${_include_dirs}")

add_library(camera_model
    src/chessboard/Chessboard.cc
    src/calib/CameraCalibration.cc
    src/camera_models/Camera.cc
    src/camera_models/CameraFactory.cc
    src/camera_models/CostFunctionFactory.cc
    src/camera_models/PinholeCamera.cc
    src/camera_models/CataCamera.cc
    src/camera_models/EquidistantCamera.cc
    src/camera_models/ScaramuzzaCamera.cc
    src/sparse_graph/Transform.cc
    src/gpl/gpl.cc
    src/gpl/EigenQuaternionParameterization.cc
)

add_executable(Calibration 
    src/intrinsic_calib.cc
    src/chessboard/Chessboard.cc
    src/calib/CameraCalibration.cc
    src/camera_models/Camera.cc
    src/camera_models/CameraFactory.cc
    src/camera_models/CostFunctionFactory.cc
    src/camera_models/PinholeCamera.cc
    src/camera_models/CataCamera.cc
    src/camera_models/EquidistantCamera.cc
    src/camera_models/ScaramuzzaCamera.cc
    src/sparse_graph/Transform.cc
    src/gpl/gpl.cc
    src/gpl/EigenQuaternionParameterization.cc
)

target_link_libraries(Calibration ${OpenCV_LIBS} ${Boost_LIBRARIES} ${CERES_LIBRARIES} ${catkin_LIBRARIES})
target_link_libraries(camera_model ${OpenCV_LIBS} ${Boost_LIBRARIES} ${CERES_LIBRARIES} ${catkin_LIBRARIES})
