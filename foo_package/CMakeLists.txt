cmake_minimum_required(VERSION 3.8)
project(foo_package)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(bar_package CONFIG REQUIRED)
message(${bar_package_DIR})

add_executable(foo_node src/foo_node.cpp)
target_include_directories(foo_node PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(foo_node PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17

# This works
# ament_target_dependencies(
#   foo_node
#   "bar_package"
# )

# This doesn't work
# target_link_libraries(
#   foo_node
#   bar_package
# )

# This does work, but is super ugly
# target_link_libraries(
#   foo_node
#   bar_package::bar_package__rosidl_generator_cpp
# )

# Why can't I just link the library as an ALIAS like so? This target doesn't exist
# target_link_libraries(
#   foo_node
#   bar_package::bar_package
# )

install(TARGETS foo_node
  DESTINATION lib/${PROJECT_NAME})

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

ament_package()
