cmake_minimum_required(VERSION 3.10)
cmake_policy(SET CMP0079 NEW)
project(dasher VERSION 0.0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# add_compile_options(-rdynamic)
add_link_options(-rdynamic)

option(ENABLE_NLS "Enable native language support" ON)
option(WITH_DEBUG "Build with additional debugging checks" OFF)
option(WITH_GNOME "Build GNOME documentation" ON)
option(WITH_SPEECH "Build with speech support" ON)
option(WITH_ATSPI "Use AT-SPI rather than Xtst for direct entry mode if found" ON)
option(WITH_JAPANESE "Build with support for Japanese Kanji entry (experimental)" OFF)
option(WITH_JOYSTICK "Build with Linux joystick support" OFF)
option(WITH_TILT "Build with experimental support for tilt sensor input" OFF)

include(CheckIncludeFiles)
include(CheckFunctionExists)
include(CheckSymbolExists)
include(CheckLibraryExists)
include(GNUInstallDirs)



check_include_files("dlfcn.h" HAVE_DLFCN_H)
check_include_files("inttypes.h" HAVE_INTTYPES_H)
check_include_files("unistd.h" HAVE_UNISTD_H)
check_include_files("sys/stat.h" HAVE_SYS_STAT_H)
check_function_exists("round" HAVE_ROUND)
check_function_exists("lldiv" HAVE_LLDIV)
check_library_exists(expat XML_ParserCreate "" HAVE_LIBEXPAT)
check_library_exists(Xtst XTestFakeKeyEvent "" HAVE_LIBXTST)

# Additional library checks might be more involved, requiring custom logic
find_package(PkgConfig)
pkg_check_modules(SPEECHD QUIET libspeechd)
if(SPEECHD_FOUND)
    set(HAVE_SPEECHD 1)
endif()



include_directories(${CMAKE_BINARY_DIR})

find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK REQUIRED gtk+-3.0)
pkg_check_modules(GIO REQUIRED gio-2.0)
pkg_check_modules(GLIB REQUIRED glib-2.0)
pkg_check_modules(ATSPI REQUIRED atspi-2)

if (GTK_FOUND)
    set(HAVE_GTK 1)
endif()
if(GIO_FOUND)
    set(HAVE_GIO 1)
endif()
if(GLIB_FOUND)
    set(HAVE_GLIB 1)
endif()

set(HAVE_CONFIG_H 1)

# For backwards compatibility with autoconf wrt file globbing
add_compile_definitions(CMAKE_BUILD)


add_subdirectory(Src)
add_subdirectory(Data)
add_subdirectory(Doc)
add_subdirectory(po)



add_compile_options(${GTK_CFLAGS_OTHER})  # Proper way to add compile options from pkg-config

# Global include and link directories are still necessary for pkg-config managed packages
include_directories(${GTK_INCLUDE_DIRS} ${GIO_INCLUDE_DIRS} ${GLIB_INCLUDE_DIRS})
link_directories(${GTK_LIBRARY_DIRS} ${GIO_LIBRARY_DIRS} ${GLIB_LIBRARY_DIRS})

message(STATUS "GLIB_INCLUDE_DIRS: ${GLIB_INCLUDE_DIRS}")

check_include_files("stdint.h" HAVE_STDINT_H)
check_include_files("stdio.h" HAVE_STDIO_H)
check_include_files("stdlib.h" HAVE_STDLIB_H)
check_include_files("strings.h" HAVE_STRINGS_H)
check_include_files("string.h" HAVE_STRING_H)
check_include_files("sys/types.h" HAVE_SYS_TYPES_H)


set (CMAKE_REQUIRED_INCLUDES ${GTK_INCLUDE_DIRS})
check_include_files("gdk/gdkkeysyms-compat.h" HAVE_GDK_GDKKEYSYMS_COMPAT_H)
set (CMAKE_REQUIRED_INCLUDES ${GTK_INCLUDE_DIRS})
check_symbol_exists("gtk_show_uri_on_window" "gtk/gtkshow.h" HAVE_GTK_SHOW_URI_ON_WINDOW)
message(STATUS "HAVE_GDK_GDKKEYSYMS_COMPAT_H: ${HAVE_GDK_GDKKEYSYMS_COMPAT_H}")



set(HAVE_GETTEXT 1)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in ${CMAKE_BINARY_DIR}/config.h)