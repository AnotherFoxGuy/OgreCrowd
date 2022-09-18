cmake_minimum_required(VERSION 3.4.3)
project(OgreCrowd)

include(${CMAKE_BINARY_DIR}/conanbuildinfo_multi.cmake)
conan_basic_setup(TARGETS)

option(BUILD_DEMO "Build demo" OFF)

# recast lib versions
SET(SOVERSION 1)
SET(VERSION 1.0.0)

include_directories(include Samples/include fastlz)

file(GLOB ogrecrowdcpp "${PROJECT_SOURCE_DIR}/src/*.cpp" "${PROJECT_SOURCE_DIR}/fastlz/fastlz.c")

add_library(OgreCrowd ${ogrecrowdcpp})
target_link_libraries(OgreCrowd PUBLIC CONAN_PKG::OGRE CONAN_PKG::ois CONAN_PKG::recastnavigation)

INSTALL(TARGETS OgreCrowd
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib)

INSTALL(DIRECTORY ${CMAKE_SOURCE_DIR}/include/ DESTINATION include FILES_MATCHING PATTERN "*.h")

if(BUILD_DEMO)
  file(COPY OgreRecast.cfg DESTINATION ${PROJECT_BINARY_DIR})
  configure_file(resources.cfg.in ${PROJECT_BINARY_DIR}/resources.cfg)

  file(GLOB democpp "${PROJECT_SOURCE_DIR}/Samples/src/*.cpp")
  add_executable(OgreCrowdDemo ${democpp})
  target_link_libraries(OgreCrowdDemo PUBLIC OgreCrowd)
endif()