# Specify the directory where the compiled library will be built
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)

# Include the directory of DasherCore for header file access
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../DasherCore)

# Define a static library target named libdashermisc
add_library(libdashermisc STATIC
    AppSettingsData.h
    AppSettingsHeader.h
    Common.h
    Globber.h
    Globber.cpp
    I18n.h
    MSVC_Unannoy.h
    myassert.h
    mydebug.h
    ModuleSettings.h
    NoClones.h
    Trace.cpp
    Trace.h
    Allocators/PooledAlloc.h
    Allocators/SimplePooledAlloc.h
    Platform/stdminmax.h
    Types/int.h
)

# Set property to not install this library since it's for internal use only (noinst)
set_target_properties(libdashermisc PROPERTIES EXCLUDE_FROM_ALL TRUE)

# Optional, distribute these extra files in the project (if using CPack or similar)
set_property(SOURCE Common.vcxproj PROPERTY MACOSX_PACKAGE_LOCATION "Resources")
