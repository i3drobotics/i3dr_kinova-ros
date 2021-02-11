cmake_minimum_required(VERSION 2.8.3)
set (CMAKE_CXX_STANDARD 11)
project(i3dr_kinova)

find_package(catkin REQUIRED COMPONENTS
                roscpp std_msgs tf rospy std_srvs cv_bridge image_transport message_filters message_generation sensor_msgs rtabmap_ros
            ) 
find_package(OpenCV REQUIRED)

include_directories(
    ${catkin_INCLUDE_DIRS}
)

catkin_package(
    #INCLUDE_DIRS include
	CATKIN_DEPENDS roscpp std_msgs
	LIBRARIES ${PROJECT_NAME}
)