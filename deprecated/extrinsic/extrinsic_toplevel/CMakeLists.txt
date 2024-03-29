cmake_minimum_required(VERSION 3.0.2)
project(extrinsic_toplevel)

find_package(catkin REQUIRED COMPONENTS
	roscpp
	roslib
	message_generation
	geometry_msgs
	tf2_ros
)
find_package(Eigen3 REQUIRED)

add_service_files(FILES RunCalibration.srv)
generate_messages(DEPENDENCIES
	std_msgs
	geometry_msgs
)

include_directories(${catkin_INCLUDE_DIRS})
include_directories(${EIGEN3_INCLUDE_DIR})


catkin_package(
	CATKIN_DEPENDS message_runtime
)

add_executable(extrinsic_exec_service src/extrinsic_exec.cpp)
#This next line is necessary for referenceing message defs in the same package as source code: https://answers.ros.org/question/53265/catkin-messages-and-node-in-same-package/
add_dependencies(extrinsic_exec_service extrinsic_toplevel_gencpp)
target_link_libraries(extrinsic_exec_service ${catkin_LIBRARIES} yaml-cpp)

add_executable(extrinsic_publish_service src/extrinsic_publish.cpp)
#This next line is necessary for referenceing message defs in the same package as source code: https://answers.ros.org/question/53265/catkin-messages-and-node-in-same-package/
#add_dependencies(extrinsic_publish_service extrinsic_publish_gencpp)
target_link_libraries(extrinsic_publish_service ${catkin_LIBRARIES} yaml-cpp)
