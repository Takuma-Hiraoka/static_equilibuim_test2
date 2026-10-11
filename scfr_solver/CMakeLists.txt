cmake_minimum_required(VERSION 3.16)
project(scfr_solver)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(ament_cmake REQUIRED)

set(dependencies
  Eigen3
  static_equilibuim_test)

foreach(dependency ${dependencies})
  find_package(${dependency} REQUIRED)
endforeach()

add_library(${PROJECT_NAME} src/scfr_solver.cpp)

target_include_directories(${PROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_link_libraries(${PROJECT_NAME} Eigen3::Eigen static_equilibuim_test::static_equilibuim_test)

install(TARGETS ${PROJECT_NAME} EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib LIBRARY DESTINATION lib RUNTIME DESTINATION bin)
install(DIRECTORY include/ DESTINATION include FILES_MATCHING PATTERN "*.h")

add_executable(sample src/sample.cpp)
target_link_libraries(sample ${PROJECT_NAME})

install(TARGETS sample DESTINATION lib/${PROJECT_NAME})

ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_export_dependencies(${dependencies})
ament_package()
