
cmake_minimum_required(VERSION 2.8.3)
project(ipopt_example)

add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

set(IPOPT_DIR "/home/david/software/Ipopt-3.12.8")
if(IS_DIRECTORY ${IPOPT_DIR}/build/include/coin)
  set(IPOPT_INCLUDE_DIRS ${IPOPT_DIR}/build/include/coin)
  set(IPOPT_LIBRARIES    ${IPOPT_DIR}/build/lib/libipopt.so)
else()
  message(WARNING "IPOPT directory \"" ${IPOPT_DIR} "\" NOT found "
                  "-> Not compiling ifopt_ipopt. \n"
                  "Specify path to your installed IPOPT installation here.")
  return()
endif()



catkin_package(
  INCLUDE_DIRS include ${IPOPT_INCLUDE_DIRS}

)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${IPOPT_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)

add_library(ipopt_interface src/ipopt_interface.cpp)
add_executable(ipopt_example src/main.cpp)
target_link_libraries(ipopt_example ${catkin_LIBRARIES} ${IPOPT_LIBRARIES} ipopt_interface)

