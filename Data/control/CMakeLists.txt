cmake_minimum_required(VERSION 3.10)

# Define the project directory for installing data files
set(DATA_INSTALL_DIR "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}")

# List all control configuration files
set(CONTROL_FILES
    control.dtd
    control.xml
    control.411.xml
    control.all.txt.xml
    control.all.xml
    control.edit.txt.xml
    control.edit.xml
    control.speak.txt.xml
    control.speak.xml
    control.prepared_speech.txt.xml
    control.prepared_speech.xml
)

# Install control files to the appropriate data directory
install(FILES ${CONTROL_FILES}
    DESTINATION ${DATA_INSTALL_DIR}/control
)
