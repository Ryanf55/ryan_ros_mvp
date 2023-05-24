
TO reproduce
```
. /opt/ros/humble/setup.bash


# Create bar (the message package)
ros2 pkg create --build-type ament_cmake bar_package

# Create foo (the node package)
ros2 pkg create --build-type ament_cmake --dependencies bar_package --node-name foo_node foo_package
# You can see the dependency is linked with 
```

The package is linked like so, despite that being the worse way
```cmake
ament_target_dependencies(
  foo_node
  "bar_package"
)
```

If we change to use cmake like so: 
```
target_link_libraries(
  foo_node
  "bar_package"
)
```

Then, add a message, then try linking the packages together so you can use the new header.

Then, you get a build failure. 
```
/home/ryan/Development/ros2_ws/src/ryan_ros_mvp/foo_package/src/foo_node.cpp:3:10: fatal error: bar_package/msg/foo.hpp: No such file or directory
    3 | #include "bar_package/msg/foo.hpp"
```

rosidl_generate_interfaces may go away: https://github.com/ros2/rosidl/issues/400#issuecomment-1283029739

The docs recommend using modern cmake, but don't show an example linking to a message package. See here:
https://docs.ros.org/en/foxy/How-To-Guides/Ament-CMake-Documentation.html#adding-dependencies