# == DO NEVER EDIT THE NEXT LINES for Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 1.5.1)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
include(${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
# ====================================================================================
cmake_minimum_required(VERSION 3.24.1)

include(pico_sdk_import.cmake)

include(FetchContent)

project(picostation)

pico_sdk_init()

FetchContent_Declare(FatFs_SPI
        GIT_REPOSITORY https://github.com/carlk3/no-OS-FatFS-SD-SDIO-SPI-RPi-Pico.git
        SOURCE_SUBDIR src
        GIT_TAG v1.2.4
        )
FetchContent_MakeAvailable(FatFs_SPI)

add_executable(picostation)

target_sources(picostation PRIVATE main.c utils.c subq.c cmd.c hw_config.c i2s.c)

pico_enable_stdio_usb(picostation 1)

pico_generate_pio_header(picostation ${CMAKE_CURRENT_LIST_DIR}/main.pio)

target_link_libraries(picostation PRIVATE pico_stdlib hardware_pio hardware_dma FatFs_SPI pico_multicore)

pico_set_binary_type(picostation copy_to_ram)

pico_add_extra_outputs(picostation)
