cmake_minimum_required(VERSION 3.10)

# Define the project directory for installing settings data files
set(DATA_INSTALL_DIR "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}/settings")

# List all settings XML and DTD files
set(SETTINGS_FILES
    settings.dtd
    settings.direct.xml
    settings.edit.xml
    settings.prepared_speech.xml
    settings.speak.xml
)

# Install settings files to the appropriate data directory
install(FILES ${SETTINGS_FILES}
    DESTINATION ${DATA_INSTALL_DIR}
)
