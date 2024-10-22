cmake_minimum_required(VERSION 3.15)
project(robotiq_modbus_project)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(PYTHON_EXECUTABLE "C:/Users/Chris/anaconda3/python.exe")

# Include paths
include_directories(include)
include_directories(pybind11/include)

# Find Pybind11 - Assuming it's a submodule or included in the directory
add_subdirectory(pybind11)

# Define the imported shared library properly
add_library(modbus SHARED IMPORTED)
set_target_properties(modbus PROPERTIES
    IMPORTED_LOCATION "${CMAKE_SOURCE_DIR}/lib/libmodbus_build.dll"
    IMPORTED_IMPLIB "${CMAKE_SOURCE_DIR}/lib/libmodbus_build.lib"
)

# Create an executable for your application
add_executable(robotiq_modbus src/robotiq_modbus.cpp)
set_target_properties(robotiq_modbus PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}")

# Link against the imported Modbus library and Pybind11
target_link_libraries(robotiq_modbus PRIVATE modbus pybind11::pybind11 ws2_32)
