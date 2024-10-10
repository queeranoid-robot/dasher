# # Set locale directory
# set(LOCALEDIR "${CMAKE_INSTALL_DATADIR}/locale")

# # Definitions for preprocessor
# add_definitions(-DLOCALEDIR="${LOCALEDIR}")
# add_definitions(-DPREFIX="${CMAKE_INSTALL_PREFIX}")
# add_definitions(-DLIBDIR="${CMAKE_INSTALL_LIBDIR}")
# add_definitions(-DDATADIR="${CMAKE_INSTALL_DATADIR}")
# add_definitions(-DPROGDATA="${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}")
# add_definitions(-DSYSCONFDIR="${CMAKE_INSTALL_SYSCONFDIR}")
# add_definitions(-DPACKAGE_LOCALE_DIR="${LOCALEDIR}")

# # Include directories
# include_directories(
#     ${CMAKE_CURRENT_SOURCE_DIR}/Gtk2
#     ${CMAKE_CURRENT_SOURCE_DIR}/DasherCore
#     ${CMAKE_CURRENT_SOURCE_DIR}/rust/rust_test_lib/target/include
#     ${GTKBUILD_CFLAGS}
#     ${SPEECH_CPPFLAGS}
# )

# # Link directories for Rust library
# link_directories(${CMAKE_CURRENT_SOURCE_DIR}/rust/rust_test_lib/target/debug)

# # Define the executable
# add_executable(dasher main.cc)

# # Specify subdirectories to process
# add_subdirectory(Common)
# add_subdirectory(DasherCore)
# add_subdirectory(Gtk2)
# add_subdirectory(rust)

# # Linking dependencies
# target_link_libraries(dasher
#     PRIVATE
#         dashergtk
#         dashercontrol
#         dashercore
#         dasherprefs
#         dasherlm
#         rust_test_lib_static
#         ${GTKBUILD_LIBS}
#         ${SPEECH_LIBS}
#         expat
# )

# # Install the binary
# install(TARGETS dasher RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
# Set locale directory
set(LOCALEDIR "${CMAKE_INSTALL_DATADIR}/locale")
set(DASHERPROGDATA "../${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}")

# Define the executable
add_executable(dasher main.cc)

# Specify subdirectories to process
add_subdirectory(Common)
add_subdirectory(DasherCore)
add_subdirectory(Gtk2)
add_subdirectory(rust)

add_dependencies(dasher rust_test_lib_header rust_test_lib libdashercontrol libdashergtk libdashercore libdasherlm libdasherprefs)

# Include directories specific to the dasher executable
target_include_directories(dasher
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/Gtk2
        ${CMAKE_CURRENT_SOURCE_DIR}/DasherCore
        ${CMAKE_CURRENT_SOURCE_DIR}/rust/rust_test_lib/target/include
        ${GLIB_INCLUDE_DIRS} # Adding GLib include directories
        ${GTK_INCLUDE_DIRS} # Adding GTK include directories
)

# Preprocessor definitions specific to the dasher executable
target_compile_definitions(dasher
    PRIVATE
        LOCALEDIR="${LOCALEDIR}"
        PREFIX="${CMAKE_INSTALL_PREFIX}"
        LIBDIR="${CMAKE_INSTALL_LIBDIR}"
        DATADIR="${CMAKE_INSTALL_DATADIR}"
        PROGDATA="${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}"
        SYSCONFDIR="${CMAKE_INSTALL_SYSCONFDIR}"
        PACKAGE_LOCALE_DIR="${LOCALEDIR}"
)
if(WITH_SPEECH)
    find_path(SPEECHD_INCLUDE_DIR speech-dispatcher/libspeechd.h)
    find_library(SPEECHD_LIBRARY NAMES speechd)
    if(SPEECHD_INCLUDE_DIR AND SPEECHD_LIBRARY)
        add_definitions(-DHAVE_SPEECHD)
        target_include_directories(dasher PRIVATE ${SPEECHD_INCLUDE_DIR})
        target_link_libraries(dasher PRIVATE ${SPEECHD_LIBRARY})
        message(STATUS "Speech support enabled.")
        message(STATUS "SPEECHD_LIBRARY: ${SPEECHD_LIBRARY}")
    else()
        message(FATAL_ERROR "Speech support requested but speech dispatcher not found.")
    endif()
endif()

message(STATUS "HAVE_CONFIG_H: ${HAVE_CONFIG_H}")
if (HAVE_CONFIG_H)
    add_compile_definitions(HAVE_CONFIG_H)
endif()

# Link directories for Rust library
link_directories(${CMAKE_CURRENT_SOURCE_DIR}/rust/rust_test_lib/target/debug)

target_link_directories(dasher
    PRIVATE
        ${CMAKE_CURRENT_BINARY_DIR}/Gtk2 # Adding Gtk2 directory
        ${CMAKE_CURRENT_SOURCE_DIR}/DasherCore # Adding DasherCore directory
        ${CMAKE_CURRENT_SOURCE_DIR}/Common # Adding Common directory
)




# Linking dependencies
target_link_libraries(dasher
    PRIVATE
        libdashergtk
        libdashercontrol
        libdashercore
        libdasherprefs
        libdasherlm
        rust_test_lib_static
        ${GTK_LIBRARIES} # Ensure GTK libraries are correctly linked
        ${GTHREAD_LIBRARIES} # Link GThread if used
        ${SPEECH_LIBS}
        expat
        X11
        Xtst
        atspi
)

# Compiler options to ensure GTK and GLib flags are applied
target_compile_options(dasher PRIVATE ${GTK_CFLAGS_OTHER} ${GLIB_CFLAGS_OTHER})

# Install the binary
install(TARGETS dasher RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
