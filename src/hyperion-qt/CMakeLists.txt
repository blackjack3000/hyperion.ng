cmake_minimum_required(VERSION 3.0.0)
project(hyperion-qt)

find_package(Qt5Widgets REQUIRED)

include_directories(
	${CMAKE_CURRENT_BINARY_DIR}/../../libsrc/flatbufserver
	${FLATBUFFERS_INCLUDE_DIRS}
)

set(Hyperion_QT_HEADERS
	QtWrapper.h
)

set(Hyperion_QT_SOURCES
	QtWrapper.cpp
	hyperion-qt.cpp
)

add_executable(${PROJECT_NAME}
	${Hyperion_QT_HEADERS}
	${Hyperion_QT_SOURCES}
)

target_link_libraries(${PROJECT_NAME}
	commandline
	qt-grabber
	flatbufserver
	flatbuffers
	ssdp
	Qt5::Core
	Qt5::Widgets
	Qt5::Network
)

install ( TARGETS ${PROJECT_NAME} DESTINATION "share/hyperion/bin/" COMPONENT "${PLATFORM}" )

if(CMAKE_HOST_UNIX)
	install(CODE "EXECUTE_PROCESS(COMMAND ln -sf \"../share/hyperion/bin/${PROJECT_NAME}\" \"${CMAKE_BINARY_DIR}/symlink_${PROJECT_NAME}\" )" COMPONENT  "${PLATFORM}" )
	install(FILES "${CMAKE_BINARY_DIR}/symlink_${PROJECT_NAME}" DESTINATION "bin" RENAME "${PROJECT_NAME}" COMPONENT "${PLATFORM}" )
	install(CODE "FILE (REMOVE ${CMAKE_BINARY_DIR}/symlink_${PROJECT_NAME} )" COMPONENT  "${PLATFORM}" )
endif(CMAKE_HOST_UNIX)
