# Minimum CMake version required
cmake_minimum_required(VERSION 3.10)

# Project name and language
project(RISCVProject C ASM)

# Set the RISC-V toolchain
set(CMAKE_C_COMPILER riscv64-unknown-elf-gcc)
set(CMAKE_ASM_COMPILER riscv64-unknown-elf-gcc)

# Set the C standard
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED True)

# Source files
set(SOURCES main.c vec.S)

# Add the executable
add_executable(${PROJECT_NAME} ${SOURCES})

# Specify the target architecture and optimization
target_compile_options(${PROJECT_NAME} PRIVATE -O -march=rv64gcv_zba)

# Link math library
target_link_libraries(${PROJECT_NAME} m)

# Custom target to run the executable with QEMU
add_custom_target(
    run
    COMMAND qemu-riscv64 -cpu rv64,v=true,zba=true,vlen=128,rvv_ta_all_1s=on,rvv_ma_all_1s=on ${PROJECT_NAME}
    DEPENDS ${PROJECT_NAME}
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMENT "Running main with QEMU"
)
