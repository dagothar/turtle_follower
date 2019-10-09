cmake_minimum_required(VERSION 2.8.3)
project(turtlefollower)

find_package(catkin REQUIRED COMPONENTS geometry_msgs message_generation rosconsole roscpp roscpp_serialization roslib rostime std_msgs std_srvs turtlesim)

find_package(Qt5Widgets REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

#add_message_files(DIRECTORY msg FILES
#                  )
#add_service_files(DIRECTORY srv FILES
#                  )
#generate_messages(DEPENDENCIES geometry_msgs std_msgs std_srvs)

catkin_package(CATKIN_DEPENDS geometry_msgs message_runtime std_msgs std_srvs)

set(turtlefollower_node_SRCS
  src/turtlefollower_node.cpp
  src/turtle.cpp
  src/turtle_frame.cpp
)
set(turtlefollower_node_HDRS
  include/turtlefollower/turtle_frame.h
)

qt5_wrap_cpp(turtlefollower_node_MOCS ${turtlefollower_node_HDRS})


add_executable(turtlefollower_node ${turtlefollower_node_SRCS} ${turtlefollower_node_MOCS})
target_link_libraries(turtlefollower_node Qt5::Widgets ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(turtlefollower_node turtlefollower_gencpp)

install(TARGETS turtlefollower_node
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(DIRECTORY images
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  FILES_MATCHING PATTERN "*.png" PATTERN "*.svg")
