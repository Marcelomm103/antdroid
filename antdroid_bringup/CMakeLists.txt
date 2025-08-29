cmake_minimum_required(VERSION 2.8.3)

# Define o padrão C++11 para todos os alvos do projeto
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Adiciona a flag de compilação para garantir que o compilador use C++11
add_compile_options(-std=c++11)

project(antdroid_bringup)


find_package(catkin REQUIRED COMPONENTS
  roslaunch
)

catkin_package()

roslaunch_add_file_check(launch)


install(DIRECTORY launch
DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
