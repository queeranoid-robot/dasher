cmake_minimum_required(VERSION 3.10)

# Handle subdirectories
add_subdirectory(training)
add_subdirectory(alphabets)
add_subdirectory(colours)
add_subdirectory(control)
add_subdirectory(settings)
add_subdirectory(GUI)
add_subdirectory(Help)

# Set variables for directories
set(desktopdir "${CMAKE_INSTALL_DATADIR}/applications")
set(icondir "${CMAKE_INSTALL_DATADIR}/icons/hicolor/48x48/apps")
set(svgicondir "${CMAKE_INSTALL_DATADIR}/icons/hicolor/scalable/apps")

# Process the desktop file
find_program(GETTEXT_MSGFMT_EXECUTABLE msgfmt)
if(NOT GETTEXT_MSGFMT_EXECUTABLE)
    message(FATAL_ERROR "msgfmt not found")
endif()

set(desktop_in "${CMAKE_CURRENT_SOURCE_DIR}/dasher.desktop.in.in")
set(desktop_out "${CMAKE_CURRENT_BINARY_DIR}/dasher.desktop")

add_custom_command(
    OUTPUT ${desktop_out}
    COMMAND ${GETTEXT_MSGFMT_EXECUTABLE} --desktop --template ${desktop_in} -d ${top_srcdir}/po -o ${desktop_out}
    # DEPENDS ${desktop_in}
    COMMENT "Generating desktop file from template"
)

add_custom_target(GenerateDesktop ALL DEPENDS ${desktop_out})

# Installation of the desktop file and icons
install(FILES ${desktop_out} DESTINATION ${desktopdir})
install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/dasher.png" DESTINATION ${icondir})
install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/dasher.svg" DESTINATION ${svgicondir})

# Custom target to update the icon cache on install and uninstall
install(CODE "
    execute_process(COMMAND gtk-update-icon-cache -f -t ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_DATADIR}/icons/hicolor)
    message(STATUS \"Updated GTK icon cache.\")
")

# Define uninstall hook
install(CODE "
    if(NOT DEFINED ENV{DESTDIR})
        execute_process(COMMAND gtk-update-icon-cache -f -t ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_DATADIR}/icons/hicolor)
        message(STATUS \"Updated GTK icon cache on uninstall.\")
    else()
        message(STATUS \"Icon cache not updated. After uninstall, run: gtk-update-icon-cache -f -t ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_DATADIR}/icons/hicolor\")
    endif()
")

# Clean-up files
set_property(DIRECTORY PROPERTY ADDITIONAL_MAKE_CLEAN_FILES "${desktop_out}")
