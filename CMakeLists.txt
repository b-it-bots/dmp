cmake_minimum_required(VERSION 2.8.3)
project(dmp)

find_package(catkin REQUIRED COMPONENTS
roscpp
rospy
message_generation
)
find_package(cmake_modules REQUIRED)
find_package(Eigen REQUIRED)

add_message_files(
   FILES
   DMPData.msg
   DMPPoint.msg
   DMPTraj.msg
 )

add_service_files(
   FILES
   GetDMPPlan.srv
   LearnDMPFromDemo.srv
   SetActiveDMP.srv
 )

generate_messages()

catkin_package(
INCLUDE_DIRS include
CATKIN_DEPENDS common_msgs image_common message_runtime
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)

add_library(dmp src/dmp.cpp src/fourier_approx.cpp src/radial_approx.cpp src/linear_approx.cpp)
add_executable(dmp_server nodes/dmp_server.cpp)
add_dependencies(dmp ${PROJECT_NAME}_gencpp)
target_link_libraries(dmp_server dmp ${catkin_LIBRARIES} ${Eigen_LIBRARIES})
