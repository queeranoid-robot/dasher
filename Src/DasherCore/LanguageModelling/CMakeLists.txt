# Include the directory itself as a path to include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# Define the library libdasherlm
add_library(libdasherlm STATIC
    CTWLanguageModel.cpp
    CTWLanguageModel.h
    DictLanguageModel.cpp
    DictLanguageModel.h
    HashTable.cpp
    HashTable.h
    LanguageModel.h
    MixtureLanguageModel.h
    PPMLanguageModel.cpp
    PPMLanguageModel.h
    PPMPYLanguageModel.cpp
    PPMPYLanguageModel.h
    RoutingPPMLanguageModel.cpp
    RoutingPPMLanguageModel.h
    WordLanguageModel.cpp
    WordLanguageModel.h
)

# Specify that this library should not be installed
set_target_properties(libdasherlm PROPERTIES EXCLUDE_FROM_ALL TRUE)
