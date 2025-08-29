cmake_minimum_required(VERSION 3.16.3)
project(antdroid_antfirm)

# Set the C++ standard to C++11. This is the correct way to do it.
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(catkin REQUIRED COMPONENTS
  rosserial_arduino
  rosserial_client
  antdroid_msgs
  std_msgs
)

catkin_package()

# Include directories where the compiler should look for header files.
# ${catkin_INCLUDE_DIRS} already includes the devel/include path.
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${CMAKE_CURRENT_SOURCE_DIR}/lib
  ${CMAKE_CURRENT_BINARY_DIR}/ros_lib
)

# Generate the rosserial client libraries.
# This should happen after all message dependencies are found.
rosserial_generate_ros_lib(
    PACKAGE rosserial_arduino
    SCRIPT make_libraries.py
    )

# This is the target for compiling the firmware.
# This part of the code is responsible for building the actual firmware.
rosserial_configure_client(
  DIRECTORY firmware
  TOOLCHAIN_FILE ${ROSSERIAL_ARDUINO_TOOLCHAIN}
)

# This is the command that creates the executable and the upload target.
# It uses the `rosserial_arduino` library.
rosserial_add_client_target(firmware ALL)
rosserial_add_client_target(firmware upload)