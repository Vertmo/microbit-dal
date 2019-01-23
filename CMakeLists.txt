# This file is no longer auto-generated to make the repository builds with GCC
# and ARMCC no matter what.

cmake_minimum_required(VERSION 2.8.12)

enable_language(ASM)

file(GLOB MICROBIT-DAL_CPP_FILES
  "source/core/*.cpp"
  "source/types/*.cpp"
  "sources/drivers/*.cpp"
  "sources/bluetooth/*.cpp"
)

set(MICROBIT_DAL_VERSION_FLAGS "-DMICROBIT_DAL_VERSION=\\\"${MICROBIT_DAL_VERSION_STRING}\\\"")

set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${MICROBIT_DAL_VERSION_FLAGS}")

# Use the correct assembly file depending on 
if(CMAKE_COMPILER_IS_GNUCC)
  file(REMOVE "source/asm/CortexContextSwitch.s")
  configure_file("source/asm/CortexContextSwitch.s.gcc" "source/asm/CortexContextSwitch.s" COPYONLY)
else()
  file(REMOVE "source/asm/CortexContextSwitch.s")
  configure_file("source/asm/CortexContextSwitch.s.armcc" "source/asm/CortexContextSwitch.s" COPYONLY)
endif()

set(MICROBIT-DAL_S_FILES
    "source/asm/CortexContextSwitch.s"
)

include_directories(
  "inc/core"
  "inc/types"
  "inc/drivers"
  "inc/bluetooth"
  "inc/platform"
)
add_library(microbit-dal
    ${MICROBIT-DAL_CPP_FILES}
    ${MICROBIT-DAL_S_FILES}
)

# Link the other libraries
link_directories("../mbed-classic")

# And include their headers
include_directories(
  "../mbed-classic/api"
  "../mbed-classic/hal"
  "../mbed-classic/cmsis"
)

target_link_libraries(microbit-dal
    mbed-classic
    ble
    ble-nrf51822
)
