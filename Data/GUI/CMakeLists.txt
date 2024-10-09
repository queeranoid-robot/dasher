cmake_minimum_required(VERSION 3.10)

# Define the project directory for installing GUI data files
set(DATA_INSTALL_DIR "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}")
message(STATUS "GUI data files will be installed to: ${DATA_INSTALL_DIR}")

# List all GUI files
set(GUI_FILES
    dasher.preferences.ui
    dasher.traditional.ui
)

# Install GUI files to the appropriate data directory
install(FILES ${GUI_FILES}
    DESTINATION ${DATA_INSTALL_DIR}
)
