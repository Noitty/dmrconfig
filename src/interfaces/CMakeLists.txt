file(GLOB_RECURSE sources_list "*.c")

add_library(interfaces STATIC ${sources_list})

target_link_libraries(interfaces) # Specify dependency between libraries.
