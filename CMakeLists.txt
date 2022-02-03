
PROJECT(manus-openservo)
CMAKE_MINIMUM_REQUIRED(VERSION 3.1 FATAL_ERROR)

include(GNUInstallDirs)

SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})
SET(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})
SET(CMAKE_CXX_STANDARD 17)

OPTION(BUILD_MPSSE "Build with support for MPSSE devices" OFF)
option(BUILD_DEBUG "Enable debug output" OFF)

if(BUILD_DEBUG)
    add_definitions(-DMANUS_DEBUG)
    add_definitions(-DMANUS_SOURCE_COMPILE_ROOT="${CMAKE_CURRENT_SOURCE_DIR}/src/")
endif()

FIND_PACKAGE(echolib REQUIRED)
FIND_PACKAGE(OpenServo REQUIRED)
FIND_PACKAGE(yaml-cpp REQUIRED)
FIND_PACKAGE(manus REQUIRED)

IF (BUILD_MPSSE)
    FIND_PACKAGE(LibFTDI1 REQUIRED)
    INCLUDE_DIRECTORIES(${LIBFTDI_INCLUDE_DIRS})
ENDIF(BUILD_MPSSE)

ADD_EXECUTABLE(manus_openservo_manipulator src/main.cpp)
TARGET_LINK_LIBRARIES(manus_openservo_manipulator ${manus_LIBRARIES} ${echolib_LIBRARIES} ${OpenServo_LIBRARIES} ${YAML_CPP_LIBRARIES} ${LIBFTDI_LIBRARIES})

ADD_EXECUTABLE(manus_openservo_calibration src/calibration.cpp)
TARGET_LINK_LIBRARIES(manus_openservo_calibration ${OpenServo_LIBRARIES} ${YAML_CPP_LIBRARIES} ${LIBFTDI_LIBRARIES})

install(TARGETS 
        manus_openservo_manipulator
        manus_openservo_calibration 
        DESTINATION ${CMAKE_INSTALL_BINDIR})

