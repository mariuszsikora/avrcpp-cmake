
set(CMAKE_SYSTEM_NAME Generic)
cmake_minimum_required(VERSION 2.8)

project(TestAvrCmake)
set(MCU atmega8)
set(FCPU 8000000UL)

file(GLOB SOURCES *.cpp)

set(CMAKE_CXX_COMPILER avr-g++)
set(CMAKE_CXX_FLAGS "-Wall -Os -mmcu=${MCU} -DF_CPU=${FCPU}")
set(FLASH_FLAGS "-R .eeprom -R .fuse -R .lock -R .signature")

add_executable(${PROJECT_NAME}.elf ${SOURCES})

add_custom_target(flash ALL
    DEPENDS ${PROJECT_NAME}.elf
    COMMAND avr-objcopy ${FLASH_FLAGS} -O ihex ${PROJECT_NAME}.elf ${PROJECT_NAME}.hex
    )

set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES ${PROJECT_NAME}.hex)
