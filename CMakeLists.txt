cmake_minimum_required(VERSION 2.8.3)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

find_package(Qt4 REQUIRED)

include(${QT_USE_FILE})
include_directories(include 
  ${QT_INCLUDE_DIR} )
add_definitions(${QT_DEFINITIONS})

set(QT_USE_QTCORE TRUE)
set(QT_USE_QTGUI TRUE)

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(src)
file (GLOB UI_FILES
  ui/console_window.ui)
file (GLOB HEADER_FILES
  include/swri_console/console_master.h
  include/swri_console/console_window.h
  include/swri_console/log_database.h
  include/swri_console/node_list_model.h
  include/swri_console/log_database_proxy_model.h
  include/swri_console/ros_thread.h)
file (GLOB SRC_FILES
  src/console_master.cpp
  src/console_window.cpp
  src/log_database.cpp
  src/node_list_model.cpp
  src/log_database_proxy_model.cpp
  src/ros_thread.cpp)
QT4_ADD_RESOURCES(RCC_SRCS resources/images.qrc)
QT4_WRAP_UI(SRC_FILES ${UI_FILES})
QT4_WRAP_CPP(SRC_FILES ${HEADER_FILES})

rosbuild_add_executable(swri_console ${HEADER_FILES} ${SRC_FILES} ${RCC_SRCS} src/main.cpp)
target_link_libraries(${PROJECT_NAME} 
  ${QT_LIBRARIES})
