cmake_minimum_required(VERSION 3.8)
project(faruk)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(Qt5 COMPONENTS Widgets REQUIRED)

set(dependencies "geometry_msgs" "rclcpp" "sensor_msgs")
set(ROBOT_MOVE_NAME cpp_robot_controller)
add_executable(${ROBOT_MOVE_NAME} src/cpp_robot_controller.cpp)
target_link_libraries(${ROBOT_MOVE_NAME} Qt5::Widgets)
ament_target_dependencies(${ROBOT_MOVE_NAME} ${dependencies})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

install(TARGETS
  ${ROBOT_MOVE_NAME}
  DESTINATION lib/${PROJECT_NAME}
)

# Install launch files.
install(DIRECTORY
  launch world models config rviz
  DESTINATION share/${PROJECT_NAME}
)

ament_package()