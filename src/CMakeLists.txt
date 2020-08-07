add_subdirectory(utils)
add_subdirectory(usb_drivers)
add_subdirectory(interfaces)
add_subdirectory(core)

# Create the executable related to the main.c
add_executable(dmrconfig main.c)
target_link_libraries(dmrconfig
    utils
    usb_drivers
    interfaces
    core
)

set_target_properties(dmrconfig
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY    "${CMAKE_BINARY_DIR}/../lib"
    LIBRARY_OUTPUT_DIRECTORY    "${CMAKE_BINARY_DIR}/../lib"
    RUNTIME_OUTPUT_DIRECTORY    "${CMAKE_BINARY_DIR}/../bin"
    COMPILE_FLAGS               "${CMAKE_C_FLAGS}"
    LINK_FLAGS                  "${PROJECT_LDFLAGS}"
)
