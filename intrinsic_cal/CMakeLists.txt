cmake_minimum_required(VERSION 2.8.3)
project(intrinsic_cal)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  industrial_extrinsic_cal
  message_generation
  rosconsole
  roscpp
  roslib
  std_msgs
  std_srvs
)

find_package(Ceres REQUIRED)
message("-- Found Ceres version ${CERES_VERSION}: ${CERES_INCLUDE_DIRS}")

#set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-fPIC")


add_service_files(
  FILES
    rail_ical_run.srv
)

generate_messages(
  DEPENDENCIES
    geometry_msgs
    std_msgs
)


catkin_package(
  CATKIN_DEPENDS
    geometry_msgs
    industrial_extrinsic_cal
    message_runtime
    rosconsole
    roscpp
    roslib
    std_msgs
    std_srvs
  DEPENDS
    CERES
)


include_directories(
  ${catkin_INCLUDE_DIRS}
  ${CERES_INCLUDE_DIRS}
)


add_executable(rail_ical src/rail_cal.cpp)
add_dependencies(rail_ical ${PROJECT_NAME}_generate_messages_cpp)
target_link_libraries(rail_ical ${catkin_LIBRARIES} ${CERES_LIBRARIES})


install(
  TARGETS
    rail_ical
  RUNTIME DESTINATION
    ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  DIRECTORY
    launch
  DESTINATION
    ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(
  FILES
    asus_ir_cal.rviz
  DESTINATION
    ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
