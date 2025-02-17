cmake_minimum_required(VERSION 3.5)
project(nlohmann_json LANGUAGES CXX)

# Create an interface library
add_library(nlohmann_json INTERFACE)
add_library(nlohmann_json::nlohmann_json ALIAS nlohmann_json)

# Set include directories: use BUILD_INTERFACE for in-tree builds and INSTALL_INTERFACE for installed files
target_include_directories(nlohmann_json
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Set required compile features
if(${CMAKE_VERSION} VERSION_LESS "3.8.0")
  target_compile_features(nlohmann_json INTERFACE cxx_range_for)
else()
  target_compile_features(nlohmann_json INTERFACE cxx_std_11)
endif()

# Export targets for downstream usage
ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_package()

# Install header files
install(
  DIRECTORY include/
  DESTINATION include
)

# Install the library target and export file
install(
  TARGETS nlohmann_json
  EXPORT export_${PROJECT_NAME}
  INCLUDES DESTINATION include
)

install(
  EXPORT export_${PROJECT_NAME}
  NAMESPACE nlohmann_json::
  DESTINATION share/${PROJECT_NAME}/cmake
)
