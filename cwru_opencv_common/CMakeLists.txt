cmake_minimum_required(VERSION 2.8.3)
project(cwru_opencv_common)
find_package(OpenCV REQUIRED)

find_package(rostest REQUIRED)

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  camera_info_manager
  roscpp
  sensor_msgs
  std_msgs
  tf
  roslint
)

set(CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS}")
SET(LIB_TYPE STATIC) 

roslint_cpp()

add_service_files(DIRECTORY srv  
FILES image_label.srv
  )

#
# Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  std_msgs  
  geometry_msgs
  sensor_msgs
)

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES projective_lib
    CATKIN_DEPENDS cv_bridge image_transport roscpp sensor_msgs std_msgs
    DEPENDS OpenCV
)




include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(projective_lib
    src/projective_geometry.cpp
)

target_link_libraries(projective_lib
   ${OpenCV_LIBRARIES}
   ${catkin_LIBRARIES}
 )


# gtesting for the projective geometry library:

catkin_add_gtest(gtest_point_array_projection test/gtest_point_array_projection.cpp)


target_link_libraries(gtest_point_array_projection
       projective_lib
       ${OpenCV_LIBRERIES}
       ${catkin_LIBRARIES})


#Add rostesting
        
#add_rostest_gtest(rostest_point_array_projection test/gtest_point_array_projection.test test/gtest_point_array_projection.cpp)

#target_link_libraries(rostest_point_array_projection
#      projective_lib
#      ${OpenCV_LIBRERIES}
#      ${catkin_LIBRARIES})
