file(GLOB_RECURSE sources_list "*.c")

add_library(others STATIC ${sources_list})

target_link_libraries(others) # Specify dependency between libraries.
