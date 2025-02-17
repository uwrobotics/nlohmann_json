cmake_minimum_required(VERSION 3.5)
project(nlohmann_json LANGUAGES CXX)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
# Create an INTERFACE library target for this header-only library.
add_library(nlohmann_json INTERFACE)
add_library(nlohmann_json::nlohmann_json ALIAS nlohmann_json)

# Set up include directories for in-tree builds and installed usage.
target_include_directories(nlohmann_json
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Set required compile features.
if(${CMAKE_VERSION} VERSION_LESS "3.8.0")
  target_compile_features(nlohmann_json INTERFACE cxx_range_for)
else()
  target_compile_features(nlohmann_json INTERFACE cxx_std_11)
endif()

# If available, export targets for downstream packages.
if(COMMAND ament_export_targets)
  ament_export_targets(nlohmann_json_targets HAS_LIBRARY_TARGET)
endif()

# Standard ament package macros.
ament_package()

# Install the headers.
install(
  DIRECTORY include/
  DESTINATION include
)

# Install the library target and export file.
install(
  TARGETS nlohmann_json
  EXPORT nlohmann_json_targets
  INCLUDES DESTINATION include
)

install(
  EXPORT nlohmann_json_targets
  NAMESPACE nlohmann_json::
  DESTINATION share/${PROJECT_NAME}/cmake
)
