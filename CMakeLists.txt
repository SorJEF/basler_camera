cmake_minimum_required(VERSION 2.8.3)
project(basler_camera)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  image_transport
  camera_info_manager
  cv_bridge
  dynamic_reconfigure
)
find_package(OpenCV REQUIRED)

generate_dynamic_reconfigure_options(
  cfg/usb.cfg
)

catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES basler_camera
#  CATKIN_DEPENDS roscpp
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Additional Linker flags for pylon
SET( CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} -Wl,-E")

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include ${catkin_INCLUDE_DIRS} $ENV{PYLON_ROOT}/include $ENV{GENICAM_ROOT_V2_3}/library/CPP/include)
link_directories(
  $ENV{PYLON_ROOT}/lib
  $ENV{GENICAM_ROOT_V2_3}/bin/Linux64_x64
  $ENV{GENICAM_ROOT_V2_3}/bin/Linux64_x64/GenApi/Generic
  $ENV{GENICAM_ROOT_V2_3}/library/CPP/lib/Linux64_x64
)

## Declare a C++ library
#add_library(basler_camera src/basler_camera_node.cpp)
#add_dependencies(basler_camera ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
add_executable(basler_camera_node src/basler_camera_node.cpp)
add_dependencies(basler_camera_node ${PROJECT_NAME}_gencfg)
add_dependencies(basler_camera_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
target_link_libraries(basler_camera_node
  ${OpenCV_LIBS}
  ${catkin_LIBRARIES}
  pylonbase
  pylonutility
  GCBase_gcc_v3_0_Basler_pylon_v5_0
  GenApi_gcc_v3_0_Basler_pylon_v5_0
)
