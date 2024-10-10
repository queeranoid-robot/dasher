cmake_minimum_required(VERSION 3.10)

# Set variables for help files and directories
set(HELP_ID "dasher")
set(HELP_FILES "dasher.xml")
set(HELP_MEDIA 
    "figures/figure2.png"
    "figures/main.png"
    "figures/prefs1.png"
    "figures/prefs2.png"
    "figures/prefs3.png"
    "figures/prefs4.png"
    "figures/prefs5.png"
    "figures/a-z.gif"
    "figures/aa-az.gif"
    "figures/ala-alz.gif"
    "figures/object.gif"
    "figures/exampleHad2.png"
    "figures/exampleHad2a.png"
    "figures/exampleHad3a.png"
    "figures/exampleHad4.png"
)
set(HELP_LINGUAS ca cs de el en_GB es eu fr oc ro ru sl sv zh_CN)

# Define the installation directory for help files
set(HELP_DIR "${CMAKE_INSTALL_DATADIR}/gnome/help/${HELP_ID}")

# Install the main help file
install(FILES ${HELP_FILES} DESTINATION ${HELP_DIR})

# Install media files associated with the help files
foreach(MEDIA_FILE ${HELP_MEDIA})
    install(FILES ${MEDIA_FILE} DESTINATION "${HELP_DIR}/figures")
endforeach()

# Handle localization of help files
foreach(LANG ${HELP_LINGUAS})
    install(FILES "${LANG}/${HELP_FILES}" DESTINATION "${HELP_DIR}/${LANG}")
    # Optionally install localized media files if available
    foreach(MEDIA_FILE ${HELP_MEDIA})
        if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/${LANG}/${MEDIA_FILE}")
            install(FILES "${LANG}/${MEDIA_FILE}" DESTINATION "${HELP_DIR}/${LANG}/figures")
        endif()
    endforeach()
endforeach()

# Custom target to remind to update help files or generate them from sources
add_custom_target(update_help_files ALL
    COMMAND ${CMAKE_COMMAND} -E echo "Remember to update help files if sources have changed."
    COMMENT "Generating Help Files"
    USES_TERMINAL
)
