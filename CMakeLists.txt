# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

set(PICO_EXTRAS_PATH "E:/pico-extras")

# == DO NEVER EDIT THE NEXT LINES for Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.0.0)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
include(${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
# ====================================================================================
set(PICO_BOARD pico2 CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

# We also need PICO EXTRAS
include(pico_extras_import.cmake)

project(pico2-emu2413 C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(pico2-emu2413 pico2-emu2413.cpp
audio_i2s/picoI2sAudio.cpp audio_i2s/audio_i2s.c
emu2413_picoI2sAudioDriver.cpp emu2413.cpp
miditones/miditones.cpp miditones/midiFreqTable.cpp miditones/miditonesFile.cpp
)

pico_generate_pio_header(pico2-emu2413 ${CMAKE_CURRENT_LIST_DIR}/audio_i2s/audio_i2s.pio)

pico_set_program_name(pico2-emu2413 "pico2-emu2413")
pico_set_program_version(pico2-emu2413 "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(pico2-emu2413 1)
pico_enable_stdio_usb(pico2-emu2413 0)

# Add the standard library to the build
target_link_libraries(pico2-emu2413
        pico_double
        pico_stdlib
        pico_audio
        pico_multicore)

# Add the standard include files to the build
target_include_directories(pico2-emu2413 PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  ${CMAKE_CURRENT_LIST_DIR}/.. # for our common lwipopts or any other standard includes, if required
)

target_compile_definitions(pico2-emu2413 PRIVATE PICO_AUDIO_I2S_MONO_INPUT)

target_compile_options(pico2-emu2413 PRIVATE -O3)

# Add any user requested libraries
target_link_libraries(pico2-emu2413 
        hardware_dma
        hardware_pio
        hardware_interp
        )

pico_add_extra_outputs(pico2-emu2413)

