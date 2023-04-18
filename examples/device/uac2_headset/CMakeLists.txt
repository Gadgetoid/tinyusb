cmake_minimum_required(VERSION 3.5)
include(${CMAKE_CURRENT_SOURCE_DIR}/../../../hw/bsp/family_support.cmake)

# gets PROJECT name for the example (e.g. <BOARD>-<DIR_NAME>)
family_get_project_name(PROJECT ${CMAKE_CURRENT_LIST_DIR})

project(${PROJECT})

# Checks this example is valid for the family and initializes the project
family_initialize_project(${PROJECT} ${CMAKE_CURRENT_LIST_DIR})

target_compile_options(pico_audio_i2s INTERFACE $<$<CXX_COMPILER_ID:GNU>:-Wno-conversion -Wno-missing-field-initializers -Wno-cast-align -Wno-shadow>)

add_executable(${PROJECT})

add_compile_definitions(${PROJECT}
        NDEBUG
        PICO_AUDIO_I2S_DMA_IRQ=0
        PICO_AUDIO_I2S_PIO=0
        PICO_AUDIO_I2S_MONO_OUTPUT=0
        PICO_AUDIO_I2S_MONO_INPUT=0
        PICO_AUDIO_I2S_DATA_PIN=14
        PICO_AUDIO_I2S_CLOCK_PIN_BASE=15
)

# Example source
target_sources(${PROJECT} PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/src/main.c
        ${CMAKE_CURRENT_SOURCE_DIR}/src/i2s_audio.c
        ${CMAKE_CURRENT_SOURCE_DIR}/src/usb_descriptors.c
        )

# Example include
target_include_directories(${PROJECT} PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        )

# Configure compilation flags and libraries for the example... see the corresponding function
# in hw/bsp/FAMILY/family.cmake for details.
family_configure_device_example(${PROJECT})

target_link_libraries(${PROJECT} PUBLIC pico_stdlib pico_audio_i2s pico_unique_id pico_multicore)