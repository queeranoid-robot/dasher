cmake_minimum_required(VERSION 3.10)

# Define the data directory for installing color files
set(DATA_INSTALL_DIR "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}")

# List all color XML files
set(COLOR_FILES
    colour.dtd
    colour.blue.xml
    colour.euroasian.xml
    colour.euroasian2.xml
    colour.euroasian3.xml
    colour.jamie.xml
    colour.rainbow.xml
    colour.thai.xml
    colour.vowels.xml
    colour.vowels2.xml
    colour.xml
)

# Install color files to the appropriate data directory
install(FILES ${COLOR_FILES}
    DESTINATION ${DATA_INSTALL_DIR}/colours
)
