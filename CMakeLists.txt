cmake_minimum_required(VERSION 2.8)
project (libnpengine)

set (CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/Modules")
find_package(SFML 2 REQUIRED COMPONENTS system window graphics)
find_package(Boost REQUIRED COMPONENTS locale thread)
find_package(GStreamer REQUIRED)
find_package(GLIB REQUIRED)

add_definitions(-std=c++11)

set(CMAKE_BUILD_TYPE Debug)

add_subdirectory(externals/libnpa)

include_directories(
  ${SFML_INCLUDE_DIR}
  ${GSTREAMER_VIDEO_INCLUDE_DIRS}
  ${GLIB_INCLUDE_DIRS}
  ${PROJECT_SOURCE_DIR}/include
  ${PROJECT_SOURCE_DIR}/externals/libnpa/include
  )

aux_source_directory(${PROJECT_SOURCE_DIR}/src/ LIBNPENGINE_SRC)
link_directories(${Boost_LIBRARY_DIRS})

add_library(npengine SHARED ${LIBNPENGINE_SRC})
target_link_libraries(npengine 
  ${Boost_LIBRARIES} 
  ${SFML_LIBRARIES} 
  ${GSTREAMER_APP_LIBRARIES} 
  npa)

set_target_properties(npengine PROPERTIES LINKER_LANGUAGE CXX)
