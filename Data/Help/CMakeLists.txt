cmake_minimum_required(VERSION 3.10)

# Conditionally include the Gnome subdirectory if GNOME_HELP is set
if(GNOME_HELP)
    add_subdirectory(Gnome)
endif()

# Set variable for extra files to be included in the project distribution
set(EXTRA_DIST
    Manual.vcxproj
    compile_help.cmd
)

# While EXTRA_DIST has no direct CMake equivalent as it is for distribution only,
# you might want to keep these files listed in your project for reference or inclusion
# in source packages. Here's how you can do it in CMake:

# Print message or log the extra distribution files; actual installation or packaging
# actions depend on specific project requirements or packaging scripts.
foreach(EXTRA_FILE ${EXTRA_DIST})
    message(STATUS "Extra dist file: ${EXTRA_FILE} included in the project for reference.")
endforeach()

# Optionally, you could install these files if needed somewhere with the project.
# Uncomment and modify the following line according to actual needs:
# install(FILES ${EXTRA_DIST} DESTINATION "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}/Help")
