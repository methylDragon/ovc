cmake_minimum_required(VERSION 2.8.3)
project(ovc_embedded_driver)

add_compile_options(-std=c++14 -Wall -Wpedantic)
# For debugging
# add_compile_options(-std=c++14 -fsanitize=address)

find_package(catkin REQUIRED COMPONENTS roscpp image_transport message_generation)

add_message_files(
   FILES
   ImageCorner.msg
   Metadata.msg
)

 add_service_files(
   FILES
   ConfigureFAST.srv
)

generate_messages(
  DEPENDENCIES
  std_msgs  # Or other packages containing msgs
)

catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES ovc_embedded_driver
#  CATKIN_DEPENDS other_catkin_pkg
#  DEPENDS system_lib
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}
  src/uio_driver.cpp
  src/vdma_driver.cpp
  src/i2c_driver.cpp
  src/spi_driver.cpp
  src/dma_shapeshifter.cpp
  src/image_publisher.cpp
)

add_executable(${PROJECT_NAME}_node src/ovc_embedded_driver_node.cpp)
add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME}_node
  ${catkin_LIBRARIES}
  ${PROJECT_NAME}
  i2c
  #-fsanitize=address
)
