cmake_minimum_required(VERSION 3.5)
project(ros_modbus)

set(CMAKE_CXX_STANDARD 17)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(CMAKE_BUILD_TYPE debug)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(ros_modbus_msgs REQUIRED)
find_library(MODBUS_LIBRARY NAMES modbus REQUIRED)
find_library(YAML_CPP_LIBRARY NAMES yaml-cpp REQUIRED)

# main code is a node class + standalone executable
add_executable(modbus_node src/modbus_node.cpp src/modbus_interface.cpp)
target_include_directories(modbus_node PRIVATE include)
target_link_libraries(modbus_node ${YAML_CPP_LIBRARY} ${MODBUS_LIBRARY})
ament_target_dependencies(modbus_node rclcpp std_msgs ros_modbus_msgs)

install(TARGETS modbus_node
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME})

install(DIRECTORY
  launch config
  DESTINATION share/${PROJECT_NAME}/)

ament_package()
