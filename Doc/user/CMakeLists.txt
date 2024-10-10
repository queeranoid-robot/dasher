cmake_minimum_required(VERSION 3.10)

# Define a custom command to process the .in template
set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}")
set(BIN_DIR "${CMAKE_CURRENT_BINARY_DIR}")

# Assuming PACKAGE_VERSION and docdir are set in the higher-level CMakeLists.txt or through cache variables
set(PACKAGE_VERSION "1.0")  # Change this to the actual version
set(DOCDIR "${CMAKE_INSTALL_DOCDIR}")  # Set this appropriately based on your project's documentation directory layout

# Create a custom command that generates the dasher.1 file
add_custom_command(
    OUTPUT "${BIN_DIR}/dasher.1"
    COMMAND sed -e "s/PACKAGE_VERSION/${PACKAGE_VERSION}/" -e "s:DOCDIR:${DOCDIR}:" < "${SRC_DIR}/dasher.1.in" > "${BIN_DIR}/dasher.1"
    DEPENDS "${SRC_DIR}/dasher.1.in"
    COMMENT "Generating dasher.1 from dasher.1.in"
    VERBATIM
)

# Define a custom target that depends on the generated man page
add_custom_target(generate_man ALL DEPENDS "${BIN_DIR}/dasher.1")

# Install the generated man page
install(FILES "${BIN_DIR}/dasher.1" DESTINATION "${CMAKE_INSTALL_MANDIR}/man1")

# Optional: Add the source .in file to EXTRA_DIST if distributing source packages (handled manually or via CPack)
set(EXTRA_DIST "${SRC_DIR}/dasher.1.in")
