project(woled VERSION 1.0.0 DESCRIPTION "Waveshare OLED Library")
include(GNUInstallDirs)

add_library(${PROJECT_NAME} SHARED  src/DEV_Config.c
                                    src/OLED_Driver.c
                                    src/OLED_GUI.c)

target_include_directories( ${PROJECT_NAME}
        PRIVATE ${PROJECT_SOURCE_DIR}/include)

set_target_properties(${PROJECT_NAME} PROPERTIES
        VERSION ${PROJECT_VERSION}
        SOVERSION 1
        PUBLIC_HEADER include/oledmod/OLED_GUI.h)

configure_file(woled.pc.in woled.pc @ONLY)



install(TARGETS ${PROJECT_NAME}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

target_link_libraries(${PROJECT_NAME} mod::fonts ${WIREPI})

install(FILES ${CMAKE_BINARY_DIR}/oled/woled.pc
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)