# cmake_minimum_required(VERSION 2.8)
# set (project po)

# set(${project}_SOURCES
# 	${CMAKE_CURRENT_SOURCE_DIR}/en@boldquot.header
# 	${CMAKE_CURRENT_SOURCE_DIR}/en@quot.header
# )
# add_library(${project} STATIC ${${project}_SOURCES} )


cmake_minimum_required(VERSION 3.10)

# Define the directory where the translation files will be stored
set(LOCALE_DIR "${CMAKE_INSTALL_LOCALEDIR}")

# Gettext package details (adjust as necessary)
set(PACKAGE "your_package_name")
set(PACKAGE_VERSION "your_package_version")
set(MSGID_BUGS_ADDRESS "your_contact_email_address")

# Find Gettext utilities
find_package(Gettext REQUIRED)
if (NOT GETTEXT_FOUND)
    message(FATAL_ERROR "Gettext not found.")
endif()

# List of available translations (.po files)
set(PO_FILES
    de.po
    es.po
    fr.po
    it.po
)

# Generate .gmo files and install them
foreach(PO_FILE ${PO_FILES})
    get_filename_component(LANG ${PO_FILE} NAME_WE)
    set(MO_FILE ${LANG}.mo)

    add_custom_command(
        OUTPUT ${MO_FILE}
        COMMAND ${GETTEXT_MSGFMT_EXECUTABLE} -o ${CMAKE_CURRENT_BINARY_DIR}/${MO_FILE} ${CMAKE_CURRENT_SOURCE_DIR}/${PO_FILE}
        DEPENDS ${PO_FILE}
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        COMMENT "Building message object file for ${LANG}"
        VERBATIM
    )

    add_custom_target(${LANG}_gmo ALL DEPENDS ${MO_FILE})

    install(
        FILES ${CMAKE_CURRENT_BINARY_DIR}/${MO_FILE}
        DESTINATION ${LOCALE_DIR}/${LANG}/LC_MESSAGES
        RENAME ${PACKAGE}.mo
    )
endforeach()

# Optional: Update .pot file and update .po files from .pot file
# set(POT_FILE ${PACKAGE}.pot)
# add_custom_command(
#     OUTPUT ${POT_FILE}
#     COMMAND ${GETTEXT_XGETTEXT_EXECUTABLE} --output=${POT_FILE} --keyword=_ --keyword=N_ $(Add_other_xgettext_options_here) $(list_of_all_translatable_sources)
#     COMMAND ${CMAKE_COMMAND} -E echo "Updating ${POT_FILE} from source code."
#     DEPENDS $(PO_FILES)
#     WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
#     COMMENT "Updating the POT file"
#     VERBATIM
# )

# add_custom_target(update_pot ALL DEPENDS ${POT_FILE})

# # Rule to update .po files from .pot file
# add_custom_target(update_po ALL
#     COMMAND ${CMAKE_COMMAND} -E echo "Updating PO files from ${POT_FILE}."
#     COMMAND ${GETTEXT_MSGMERGE_EXECUTABLE} --update --backup=off --no-fuzzy-matching --verbose ${PO_FILES} ${POT_FILE}
#     DEPENDS ${PO_FILES} ${POT_FILE}
#     WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
#     COMMENT "Updating PO files"
#     VERBATIM
# )
