cmake_minimum_required(VERSION 2.8.3)
project(turtle_follower)

find_package(catkin REQUIRED COMPONENTS geometry_msgs message_generation rosconsole roscpp roscpp_serialization roslib rostime std_msgs std_srvs)

find_package(Qt5Widgets REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

add_message_files(DIRECTORY msg FILES
                  Color.msg Pose.msg)
add_service_files(DIRECTORY srv FILES
                  Kill.srv
                  SetPen.srv
                  Spawn.srv
                  TeleportAbsolute.srv
                  TeleportRelative.srv)
generate_messages(DEPENDENCIES geometry_msgs std_msgs std_srvs)

catkin_package(CATKIN_DEPENDS geometry_msgs message_runtime std_msgs std_srvs)

set(turtle_follower_SRCS
  src/turtle_follower.cpp
  src/turtle.cpp
  src/turtle_frame.cpp
)
set(turtle_follower_HDRS
  include/turtle_follower/turtle_frame.h
)

qt5_wrap_cpp(turtle_follower_MOCS ${turtle_follower_HDRS})


add_executable(turtle_follower ${turtle_follower_SRCS} ${turtle_follower_MOCS})
target_link_libraries(turtle_follower Qt5::Widgets ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(turtle_follower turtle_follower_gencpp)

install(TARGETS turtle_follower
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(DIRECTORY images
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  FILES_MATCHING PATTERN "*.png" PATTERN "*.svg")
