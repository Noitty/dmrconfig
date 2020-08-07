
# Compilation in Linux
if(UNIX)
add_library(usb_drivers STATIC dfu-libusb.c hid-linux.c)
endif(UNIX)

# Compilation in Windows
if(WIN32)
add_library(usb_drivers STATIC dfu-windows.c hid-windows.c)
endif(WIN32)

# Compilation in MacOS
if(APPLE)
add_library(usb_drivers STATIC dfu-libusb.c hid-macos.c)
endif(APPLE)


target_link_libraries(usb_drivers
    utils) # Specify dependency between libraries.
