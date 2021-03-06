cmake_minimum_required(VERSION 2.8.3)
project(imu_vn_100)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  diagnostic_updater
  param_io
  cosmo_ros
  any_measurements
  any_measurements_ros
  mabi_base_msgs
  mabi_base_description
  mabi_base_description_ros
  )
find_package(Boost REQUIRED)

catkin_package(
  INCLUDE_DIRS include vncpplib/include
  LIBRARIES imu_vn_100
  CATKIN_DEPENDS
  diagnostic_updater
  roscpp
  sensor_msgs
  param_io
  cosmo_ros
  any_measurements
  any_measurements_ros
  mabi_base_msgs
  mabi_base_description
  mabi_base_description_ros
  DEPENDS Boost
)

include_directories(
  include
  vncpplib/include
  ${catkin_INCLUDE_DIRS}
)

add_library(imu_vn_100
  vncpplib/src/arch/linux/vncp_services.c
  vncpplib/src/vndevice.c
  vncpplib/src/vn100.c
  src/imu_vn_100.cpp
  )
target_link_libraries(imu_vn_100
  ${catkin_LIBRARIES}
  )
add_dependencies(imu_vn_100
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
  )

add_executable(imu_vn_100_node
  src/imu_vn_100_node.cpp)
target_link_libraries(imu_vn_100_node
  ${catkin_LIBRARIES}
  imu_vn_100
  )
add_dependencies(imu_vn_100_node
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
  )

install(TARGETS imu_vn_100 imu_vn_100_node
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  )

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h**"
  )

install(FILES vncpplib/include/vn100.h
  vncpplib/include/vncp_services.h
  vncpplib/include/vndevice.h
  vncpplib/include/vn_errorCodes.h
  vncpplib/include/vn_kinematics.h
  vncpplib/include/vn_linearAlgebra.h
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  )

install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  )
