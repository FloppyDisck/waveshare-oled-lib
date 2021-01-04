cmake_minimum_required(VERSION 3.5)
project(woled VERSION 1.0.0 LANGUAGES C)

# Library
set(WIREPI "wiringPi")
link_libraries(stdc++fs)
add_definitions(-DUSE_WIRINGPI_LIB)
#add_definitions(-DUSE_BCM2835_LIB)


set(DEFAULT_BUILD_TYPE "Release")

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to '${DEFAULT_BUILD_TYPE}' as none was specified.")
    set(CMAKE_BUILD_TYPE "${DEFAULT_BUILD_TYPE}" CACHE STRING "Choose the type of build." FORCE)
    # Set the possible values of build type for cmake-gui
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

include(GNUInstallDirs)

set(SOURCE_FILES src/font8.c src/font12.c src/font16.c src/font20.c
        src/font24.c src/OLED_DEV_Config.c src/OLED_Driver.c src/OLED_GUI.c)

add_library(${PROJECT_NAME} SHARED ${SOURCE_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
        PRIVATE src)

set_target_properties(${PROJECT_NAME} PROPERTIES
        VERSION ${PROJECT_VERSION}
        SOVERSION 1)

target_link_libraries(${PROJECT_NAME} ${WIREPI})

install(TARGETS ${PROJECT_NAME} EXPORT woledConfig
        ARCHIVE  DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY  DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME  DESTINATION ${CMAKE_INSTALL_BINDIR})

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})

install(EXPORT woledConfig DESTINATION share/woled/cmake)

export(TARGETS ${PROJECT_NAME} FILE woledConfig.cmake)