cmake_minimum_required(VERSION 3.5)
project(realtime_tools)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra)
endif()


add_library(${PROJECT_NAME} SHARED
  src/realtime_clock.cpp)


# target_link_libraries(${PROJECT_NAME} PUBLIC ${catkin_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})
target_include_directories(${PROJECT_NAME} PUBLIC ${ament_INCLUDE_DIRS})


find_package(actionlib_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(ament_cmake_ros REQUIRED)


find_package(ament_cmake_ros REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(rcl REQUIRED)
find_package(rcl_interfaces REQUIRED)
find_package(rcl_yaml_param_parser REQUIRED)
find_package(rmw REQUIRED)
find_package(rmw_implementation REQUIRED)
find_package(rosgraph_msgs REQUIRED)
find_package(rosidl_generator_cpp REQUIRED)
find_package(rosidl_typesupport_c REQUIRED)
find_package(rosidl_typesupport_cpp REQUIRED)

# Causes the visibility macros to use dllexport rather than dllimport,
# which is appropriate when building the dll but not consuming it.
target_compile_definitions(${PROJECT_NAME}
  PRIVATE "RCLCPP_BUILDING_LIBRARY")

install(
  TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

# specific order: dependents before dependencies
ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

ament_export_dependencies(ament_cmake)
ament_export_dependencies(rcl)
ament_export_dependencies(builtin_interfaces)
ament_export_dependencies(rosgraph_msgs)
ament_export_dependencies(rosidl_typesupport_cpp)
ament_export_dependencies(rosidl_typesupport_c)
ament_export_dependencies(rosidl_generator_cpp)
ament_export_dependencies(rcl_yaml_param_parser)

  
ament_add_gtest(realtime_box_tests test/realtime_box_tests.cpp)
target_link_libraries(realtime_box_tests ${PROJECT_NAME})

ament_add_gtest(realtime_buffer_tests test/realtime_buffer_tests.cpp)
target_link_libraries(realtime_buffer_tests ${PROJECT_NAME})

ament_add_gtest(realtime_clock_tests test/realtime_clock_tests.cpp)
target_link_libraries(realtime_clock_tests ${PROJECT_NAME})

 

# add_rostest_gmock(realtime_publisher_tests test/realtime_publisher.test test/realtime_publisher_tests.cpp)
# target_link_libraries(realtime_publisher_tests ${PROJECT_NAME} ${std_msgs_LIBRARIES})
# target_include_directories(realtime_publisher_tests PRIVATE ${std_msgs_INCLUDE_DIRS})


# add_rostest_gmock(realtime_server_goal_handle_tests test/realtime_server_goal_handle.test test/realtime_server_goal_handle_tests.cpp)
# target_link_libraries(realtime_server_goal_handle_tests ${PROJECT_NAME} ${actionlib_LIBRARIES})
# target_include_directories(realtime_publisher_tests PRIVATE ${actionlib_INCLUDE_DIRS})




# Install
install(DIRECTORY include/
  DESTINATION include)
 
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)
