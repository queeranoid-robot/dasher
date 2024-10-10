# Include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/../DasherCore)

if(HAVE_CONFIG_H)
    add_compile_definitions(HAVE_CONFIG_H)
endif(HAVE_CONFIG_H)

# Definitions
add_definitions(-DPROGDATA="${DASHERPROGDATA}")

# Define the libdashercontrol library
add_library(libdashercontrol STATIC
    Canvas.cpp
    Canvas.h
    DasherControl.cpp
    DasherControl.h
    FileUtils.cpp
    FileUtils.h
    GtkDasherControl.cpp
    GtkDasherControl.h
    KeyboardHelper.cpp
    KeyboardHelper.h
    Timer.cpp
    Timer.h
    Speech.h
    custom_marshal.cpp
    custom_marshal.h
    joystick_input.h
    mouse_input.h
)

target_include_directories(libdashercontrol PRIVATE ${GTK_INCLUDE_DIRS})

set(USE_SPEECHDISPATCHER 1)
# Check for speech dispatcher usage
if(USE_SPEECHDISPATCHER)
    target_sources(libdashercontrol PRIVATE SpeechDispatcher.cpp)
    target_compile_definitions(libdashercontrol PRIVATE ${SPEECH_CPPFLAGS})
    target_link_libraries(libdashercontrol PRIVATE ${SPEECH_LIBS})
    add_compile_definitions(HAVE_SPEECHD)
endif()

# Define the libdashergtk library
add_library(libdashergtk STATIC
    DasherAppSettings.cpp
    DasherAppSettings.h
    GtkDasherControl.h
    Preferences.cpp
    Preferences.h
    dasher.h
    dasher_editor_external.h
    dasher_editor_private.h
    dasher_editor.cpp
    dasher_editor.h
    dasher_lock_dialogue.cpp
    dasher_lock_dialogue.h
    dasher_main_private.h
    dasher_main.cpp
    dasher_main.h
    module_settings_window.cpp
    module_settings_window.h
)

set(USE_ATSPI 1)

# Check for AT-SPI usage
if(USE_ATSPI)
    target_sources(libdashergtk PRIVATE dasher_editor_external_atspi.cpp)
    # target_compile_definitions(libdashergtk PRIVATE ${ATSPI_CFLAGS})
    target_link_libraries(libdashergtk PRIVATE ${ATSPI_LIBS})
else()
    target_sources(libdashergtk PRIVATE dasher_editor_external_xtest.cpp)
endif()

# Set properties to prevent installation
set_target_properties(libdashercontrol libdashergtk PROPERTIES EXCLUDE_FROM_ALL TRUE)

target_include_directories(libdashergtk PRIVATE ${GTK_INCLUDE_DIRS})

# Linking GTK and other necessary libraries
target_link_libraries(libdashercontrol PRIVATE ${GTK_LIBRARIES})
target_link_libraries(libdashergtk PRIVATE ${GTK_LIBRARIES} ${GIO_LIBRARIES} ${GLIB_LIBRARIES})
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -I/usr/include/gtk-2.0")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
