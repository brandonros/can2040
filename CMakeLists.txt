# brew install --cask gcc-arm-embedded
# brew install picotool

cmake_minimum_required(VERSION 3.13)

# Check if PICO_SDK_PATH is defined
if (DEFINED ENV{PICO_SDK_PATH})
    set(PICO_SDK_PATH $ENV{PICO_SDK_PATH})
else()
    message(FATAL_ERROR "PICO_SDK_PATH environment variable is not set. Please set it to your Pico SDK path.")
endif()

# Initialize the SDK
set(PICO_PLATFORM rp2350)
include(${PICO_SDK_PATH}/external/pico_sdk_import.cmake)

project(can2040 C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the Raspberry Pi Pico SDK
pico_sdk_init()

# Create library target
add_library(can2040 STATIC
    src/can2040.c
)

# Add include directory
target_include_directories(can2040 PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Link against pico_stdlib and hardware_pio
target_link_libraries(can2040 PUBLIC
    pico_stdlib
    hardware_pio
) 