cmake_minimum_required(VERSION 3.12)
project(RTLib-Example)

set(CMAKE_CXX_STANDARD 17)

# Add the RTLib library and its headers
add_subdirectory(RTLib)
include_directories(SYSTEM RTLib)

# Since RTLib depends on libopencm3, add the libraries to the linker paths.
# The variable LIBOPENCM3_LIB_PATH is provided by the RTLib CMake script.
link_directories(${LIBOPENCM3_LIB_PATH})

# Add the executable, set the linker flags and output directory, and link your executable against RTLib.
add_executable(${PROJECT_NAME}.elf main.cpp)
set_target_properties(${PROJECT_NAME}.elf PROPERTIES
        LINK_FLAGS "${RTLIB_LINKER_FLAGS}"
        OUTPUT_NAME "${PROJECT_NAME}_${CMAKE_BUILD_TYPE}.elf")
target_link_libraries(${PROJECT_NAME}.elf RTLib)

# Add a secondary target to generate a bin file from the elf file.
add_custom_target(
        ${PROJECT_NAME}_${CMAKE_BUILD_TYPE}.bin ALL
        COMMAND ${ARM_OBJCOPY} -Obinary "${PROJECT_NAME}_${CMAKE_BUILD_TYPE}.elf" "${PROJECT_NAME}_${CMAKE_BUILD_TYPE}.bin"
        WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}
        DEPENDS ${PROJECT_NAME}.elf
)
