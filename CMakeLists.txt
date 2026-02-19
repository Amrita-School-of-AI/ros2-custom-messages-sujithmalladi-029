cmake_minimum_required(VERSION 3.8)
project(ros2_custom_msgs)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rosidl_default_generators REQUIRED)

# Generate custom messages
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/RobotStatus.msg"
)

# Add executable
add_executable(status_publisher src/status_publisher.cpp)

# Target dependencies
ament_target_dependencies(status_publisher rclcpp)

# Link the executable to the generated custom message headers
rosidl_target_interfaces(status_publisher
  ${PROJECT_NAME} "rosidl_typesupport_cpp"
)

# Install executable
install(TARGETS
  status_publisher
  DESTINATION lib/${PROJECT_NAME}
)

# Install message directories
install(DIRECTORY msg
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
