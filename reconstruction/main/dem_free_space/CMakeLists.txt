cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_FLAGS "-std=c++11 -march=native")
find_package(OpenCV COMPONENTS opencv_core opencv_imgproc opencv_highgui REQUIRED)

if(NOT CMAKE_BUILD_TYPE)
  message(STATUS "No build type selected, default to Release")
  set(CMAKE_BUILD_TYPE "release")
endif()

message(STATUS "${Boost_LIBRARIES}")

find_package(PCL 1.2 REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

# override boost lib from PCL
#set(Boost_USE_STATIC_LIBS OFF)
#set(Boost_USE_MULTITHREADED ON)
#set(Boost_USE_STATIC_RUNTIME OFF)
find_package(Boost 1.53.0 COMPONENTS program_options;)
message(STATUS "${Boost_FOUND}")
if(NOT ${Boost_FOUND})
  message(FATAL_ERROR "boost lib not found!")
endif()


file(GLOB SRC_LIST *.cc)
set(SRC_LIST ${SRC_LIST})



message(STATUS "${OpenCV_LIBS}")
message(STATUS "${Boost_LIBRARIES}")

add_subdirectory(../../../core/ libs/core/)
add_subdirectory(../../base/ libs/base/)

add_executable(free_space ${SRC_LIST})
target_link_libraries(free_space ${Boost_LIBRARIES} ${OpenCV_LIBS} ${PCL_LIBRARIES} core recon_base)
