cmake_minimum_required(VERSION 3.14.4...3.27)
project(mav_msgs)

if(${CMAKE_VERSION} VERSION_GREATER 3.27)
  cmake_policy(SET CMP0148 OLD)
endif()

find_package(ament_cmake REQUIRED)

find_package(Eigen3 REQUIRED)
find_package(rosidl_default_generators REQUIRED)

find_package(geometry_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(trajectory_msgs REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  msg/Actuators.msg
  msg/AttitudeThrust.msg
  msg/FilteredSensorData.msg
  msg/GpsWaypoint.msg
  msg/RateThrust.msg
  msg/RollPitchYawrateThrust.msg
  msg/Status.msg
  msg/TorqueThrust.msg
  DEPENDENCIES std_msgs geometry_msgs nav_msgs
)

################################################################################
# ADDED THIS SO WE CAN INCLUDE THE HEADERS IN ./include
################################################################################
add_library(${PROJECT_NAME}_conversions INTERFACE)
target_include_directories(${PROJECT_NAME}_conversions INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

install(TARGETS ${PROJECT_NAME}_conversions
        EXPORT ${PROJECT_NAME}Targets)

ament_export_targets(${PROJECT_NAME}Targets HAS_LIBRARY_TARGET)
################################################################################

ament_export_dependencies(rosidl_default_runtime)

install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}
  FILES_MATCHING PATTERN "*.hpp"
)

ament_package()
