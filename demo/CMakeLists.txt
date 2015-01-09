cmake_minimum_required(VERSION 2.8)
project(Demo)
find_package(ZLIB REQUIRED)
find_package(PNG REQUIRED)

add_definitions(${PNG_DEFINITIONS})

include_directories(
	${ZLIB_INCLUDE_DIRS}
	${PNG_INCLUDE_DIRS}
)

add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME}
	${ZLIB_LIBRARIES}
	${PNG_LIBRARIES}
)
