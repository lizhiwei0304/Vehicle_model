cmake_minimum_required(VERSION 2.8.3)
project(vehicle_demo)

set (VEHICLE_MAJOR_VERSION 0)
set (VEHICLE_MINOR_VERSION 1)
set (VEHICLE_PATCH_VERSION 0)
set (VEHICLE_VERSION_FULL ${VEHICLE_MAJOR_VERSION}.${VEHICLE_MINOR_VERSION}.${VEHICLE_PATCH_VERSION})

find_package(catkin REQUIRED COMPONENTS
  gazebo_ros
  control_msgs
)

find_package(gazebo 9 REQUIRED)
find_package(ignition-msgs1 REQUIRED)

catkin_package(
 # INCLUDE_DIRS include
 # LIBRARIES gazebo_radar_plugin
 CATKIN_DEPENDS gazebo_ros control_msgs
#  DEPENDS system_lib
)

include_directories(include ${catkin_INCLUDE_DIRS} ${GAZEBO_INCLUDE_DIRS} ${SDFormat_INCLUDE_DIRS})

add_library(VehlciePlugin SHARED plugins/VehiclePlugin.cc)
target_link_libraries(VehiclePlugin ${GAZEBO_LIBRARIES} ${IGNITION-MSGS_LIBRARIES})
add_dependencies(VehiclePlugin ${catkin_EXPORTED_TARGETS})


##catkin_install_python(PROGRAMS nodes/joystick_translator
##                      DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(TARGETS VehiclePlugin
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY models DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
#install(DIRECTORY worlds DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
