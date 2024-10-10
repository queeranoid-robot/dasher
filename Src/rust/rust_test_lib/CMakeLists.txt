# Minimum CMake version
cmake_minimum_required(VERSION 3.8)

# Project name and language (though language specification is not strictly needed as CMake won't build Rust)
project(rust_test_lib)

# Include Cargo as a custom command
set(CARGO_CMD cargo build --manifest-path ${CMAKE_CURRENT_SOURCE_DIR}/Cargo.toml)
set(TARGET_DEBUG_DIR ${CMAKE_CURRENT_SOURCE_DIR}/target/debug)

# Add custom target for Rust library
add_custom_target(rust_test_lib ALL
    COMMAND ${CARGO_CMD}
    BYPRODUCTS ${TARGET_DEBUG_DIR}/librust_test_lib.a
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMENT "Compiling Rust library rust_test_lib"
    USES_TERMINAL
)

# Set up include directory and generate header file with cbindgen
set(INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/target/include)
file(MAKE_DIRECTORY ${INCLUDE_DIR})
add_custom_command(
    OUTPUT ${INCLUDE_DIR}/rust_test_lib.h
    COMMAND cbindgen --config ${CMAKE_CURRENT_SOURCE_DIR}/cbindgen.toml --output ${INCLUDE_DIR}/rust_test_lib.h
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/src/lib.rs
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMENT "Generating C header for Rust library rust_test_lib with cbindgen"
    USES_TERMINAL
)

# Ensure header generation runs after library compilation
add_dependencies(rust_test_lib rust_test_lib_header)
add_custom_target(rust_test_lib_header ALL DEPENDS ${INCLUDE_DIR}/rust_test_lib.h)

# Add the library target manually to the project, so it can be linked with C++ code if needed
add_library(rust_test_lib_static STATIC IMPORTED GLOBAL)
set_property(TARGET rust_test_lib_static PROPERTY IMPORTED_LOCATION ${TARGET_DEBUG_DIR}/librust_test_lib.a)
set_property(TARGET rust_test_lib_static PROPERTY INTERFACE_INCLUDE_DIRECTORIES ${INCLUDE_DIR})

# Include the generated header file in the project
include_directories(${INCLUDE_DIR})

# Specify that this library should not be installed
set_target_properties(rust_test_lib_static PROPERTIES EXCLUDE_FROM_ALL TRUE)

