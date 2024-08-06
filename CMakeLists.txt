cmake_minimum_required(VERSION 3.10)
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# documentation for rosbuild macros: http://wiki.ros.org/rosbuild/CMakeLists
# rosbuild_init()


set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_STANDARD 17)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  geometry_msgs
  trajectory_msgs
  message_generation
  message_filters
)

find_package(Eigen3 REQUIRED)
find_package(Boost REQUIRED timer chrono)
find_package(yaml-cpp REQUIRED)
find_package(drift REQUIRED)

message("Eigen3 include dir: ${EIGEN3_INCLUDE_DIR}")
message("Boost include dir: ${Boost_INCLUDE_DIRS}")
message("Drift include dir: ${drift_INCLUDE_DIRS}")
message("Yaml-cpp include dir: ${YAML_CPP_INCLUDE_DIR}")
message("catkin include dir: ${catkin_INCLUDE_DIRS}")

include_directories(
  SYSTEM # suppress warnings from 3rd party libs that we can't control
  # ${PROJECT_SOURCE_DIR}/../../include
  ${PROJECT_SOURCE_DIR}/drift/include
  ${EIGEN3_INCLUDE_DIR}
  ${YAML_CPP_INCLUDE_DIR}
  ${Boost_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
  # ${drift_INCLUDE_DIRS}
)
include_directories("~/drift/include")

set(LIBS
  ${EIGEN3_LIBS}
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  ${Boost_GENERAL}
)

# Kinematic files for mini cheetah robot:
# file(GLOB MINI_CHEETAH_KIN_SRC_FILES ${PROJECT_SOURCE_DIR}/../../src/kinematics/robots/mini_cheetah/*.cpp)


set(ros_subscriber ${PROJECT_SOURCE_DIR}/drift/src/communication/ros_subscriber.cpp)
set(ros_publisher ${PROJECT_SOURCE_DIR}/drift/src/communication/ros_publisher.cpp)
add_library(ros_communication_lib
  ${ros_subscriber}
  ${ros_publisher}
)
# target_link_libraries(ros_communication_lib PUBLIC drift)
# target_include_directories(stance_leg_controller PUBLIC ${drift_INCLUDE_DIRS})

## baselines
# include_directories(
#   ${PROJECT_SOURCE_DIR}/../../baselines/include)
# add_library(baselines
#   ${PROJECT_SOURCE_DIR}/../../baselines/src/wheel_odometry/wheel_odometry.cpp)


# example for legged robot kinemtaics correction:
add_executable(mini_cheetah
  ${PROJECT_SOURCE_DIR}/drift/examples/mini_cheetah.cpp)

target_link_libraries(mini_cheetah
  ${LIBS}
  ros_communication_lib
  drift
  yaml-cpp
  ${catkin_LIBRARIES}
)

# # # temporary test for ros communicator: subscriber + publisher
# add_executable(husky
#   ${PROJECT_SOURCE_DIR}/drift/examples/husky.cpp)

# target_link_libraries(husky
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )

# # # temporary test for fetch
# add_executable(fetch
#   ${PROJECT_SOURCE_DIR}/drift/examples/fetch.cpp)

# target_link_libraries(fetch
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )

# # # temporary test for fetch
# add_executable(fetch_gyro_filter
#   ${PROJECT_SOURCE_DIR}/drift/examples/fetch_gyro_filter.cpp)

# target_link_libraries(fetch_gyro_filter
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )


# add_executable(husky_gyro_filter
#   ${PROJECT_SOURCE_DIR}/drift/examples/husky_gyro_filter.cpp)

# target_link_libraries(husky_gyro_filter
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )


# add_executable(husky_odom
#   ${PROJECT_SOURCE_DIR}/drift/examples/husky_odom.cpp)

# target_link_libraries(husky_odom
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )


# add_executable(girona500
#   ${PROJECT_SOURCE_DIR}/drift/examples/girona500.cpp)

# target_link_libraries(girona500
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )

# add_executable(husky_double_imu_filter
#   ${PROJECT_SOURCE_DIR}/drift/examples/husky_double_imu_filter.cpp)

# target_link_libraries(husky_double_imu_filter
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )

# add_executable(neya
#   ${PROJECT_SOURCE_DIR}/drift/examples/neya.cpp)

# target_link_libraries(neya
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )

# add_executable(neya_gyro_filter
#   ${PROJECT_SOURCE_DIR}/drift/examples/neya_gyro_filter.cpp)

# target_link_libraries(neya_gyro_filter
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
# )


### baselines ###
# add_executable(fetch_wheel_odom
#   ${PROJECT_SOURCE_DIR}/drift/baselines/fetch_wheel_odom.cpp)

# target_link_libraries(fetch_wheel_odom
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
#   baselines
# )

# add_executable(husky_wheel_odom
#   ${PROJECT_SOURCE_DIR}/drift/baselines/husky_wheel_odom.cpp)

# target_link_libraries(husky_wheel_odom
#   ${LIBS}
#   ros_communication_lib
#   drift
#   yaml-cpp
#   baselines
# )