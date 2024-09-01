cmake_minimum_required(VERSION 3.24.1)

include(pico_sdk_import.cmake)

include(FetchContent)

project(picostation)

pico_sdk_init()

FetchContent_Declare(no-OS-FatFS-SD-SDIO-SPI-RPi-Pico
    GIT_REPOSITORY https://github.com/carlk3/no-OS-FatFS-SD-SDIO-SPI-RPi-Pico.git
    SOURCE_SUBDIR src
    GIT_TAG 4b9fab54931c31d6422005792c430d59c43edfb3 # v3.1.0
)
FetchContent_MakeAvailable(no-OS-FatFS-SD-SDIO-SPI-RPi-Pico)

add_executable(picostation)

target_sources(picostation PRIVATE
    src/main.c
    src/utils.c
    src/subq.c
    src/cmd.c
    src/hw_config.c
    src/i2s.c
)

target_include_directories(picostation PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

pico_enable_stdio_usb(picostation 1)

pico_generate_pio_header(picostation ${CMAKE_CURRENT_LIST_DIR}/pio/main.pio)

target_link_libraries(picostation PRIVATE pico_stdlib hardware_pio hardware_dma no-OS-FatFS-SD-SDIO-SPI-RPi-Pico pico_multicore)

pico_set_binary_type(picostation copy_to_ram)

pico_add_extra_outputs(picostation)
