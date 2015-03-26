cmake_minimum_required(VERSION 2.8.3)

project(StageEnvironments)

find_package(catkin REQUIRED COMPONENTS geometry_msgs nav_msgs roscpp sensor_msgs std_msgs tf)

find_package(Boost REQUIRED COMPONENTS system thread)

find_package(FLTK REQUIRED)

if(FLTK_FOUND)
	if("${FLTK_INCLUDE_DIRS}" STREQUAL "")
		set(FLTK_INCLUDE_DIRS "${FLTK_INCLUDE_DIR}")
	endif()
	
	message(STATUS "FLTK_LIBRARIES=" ${FLTK_LIBRARIES})
	message(STATUS "FLTK_INCLUDE_DIR=" ${FLTK_INCLUDE_DIR})
	message(STATUS "FLTK_INCLUDE_DIRS=" ${FLTK_INCLUDE_DIRS})
else()
	message(FATAL_ERROR "FLTK not found...aborting!")
endif()

set(Stage_DIR cmake)
find_package(Stage REQUIRED)

message(STATUS "Stage_INCLUDE_DIRS=" ${Stage_INCLUDE_DIRS})
message(STATUS "Stage_LIBRARIES=" ${Stage_LIBRARIES})

include_directories(${catkin_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIRS})
include_directories(${FLTK_INCLUDE_DIRS})
include_directories(${Stage_INCLUDE_DIRS})

catkin_package(INCLUDE_DIRS ${Stage_INCLUDE_DIRS}
			   LIBRARIES stage_environments ${Stage_LIBRARIES}
			   CATKIN_DEPENDS geometry_msgs nav_msgs roscpp sensor_msgs std_msgs tf
			   DEPENDS Boost)

add_executable(stageros src/stageros.cpp)

target_link_libraries(stageros ${catkin_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${FLTK_LIBRARIES} ${Stage_LIBRARIES})

add_executable(stagerosPeople src/stagerosPeople.cpp)

target_link_libraries(stagerosPeople ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${FLTK_LIBRARIES} ${Stage_LIBRARIES})
