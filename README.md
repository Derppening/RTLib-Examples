# RTLib Template

Template for applications wishing to use [RTLib](https://github.com/Derppening/RTLib).

## Prerequisites

You are strongly recommended to read the [README of RTLib](https://github.com/Derppening/RTLib/blob/master/README.md) 
before proceeding.

You will be needing the following to compile:

- [arm-none-eabi-gcc](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm) (Or your preferred compiler, in that 
case provide your own toolchain file)
- [CMake](https://cmake.org/)
- [GNU Make](https://www.gnu.org/software/make/)
- [Python](https://www.python.org/)

Optional software that will make your life easier:
- [Git](https://git-scm.com/)
- [Jetbrains CLion](https://www.jetbrains.com/clion/)
- One of the following flashing tools:
    - [JLink](https://www.segger.com/products/debug-probes/j-link/technology/flash-download/)
    - [stlink](https://github.com/texane/stlink)
    - [stm32flash](https://sourceforge.net/p/stm32flash/wiki/Home/)

## Usage

1. Clone this repository to your local system. 

```bash
git clone https://github.com/Derppening/RTLib-Examples.git
```

2. Initialize all the submodule dependencies.

```bash
git submodule update --init --recursive
```

3. Build LibOpenCM3.

This step is required by RTLib to recognize which targets are supported by libopencm3. Subsequent builds can skip this 
step, as building RTLib will implicitly build libopencm3.

```bash
cd RTLib/libopencm3
make
```

4. Modify RTLib's `CMakeLists.txt`.

RTLib needs to know what MCU and board configuration your application is targeting, so modify that as necessary. 
Normally, editing the following lines should be enough:
```cmake
# Choose target device here
set(DEVICE STM32F407VET6)

# Choose target mainboard pin configuration here
set(MAINBOARD_CONFIG STM32F407_DEV)
```

If the pin configuration of your board is not identified by RTLib, you may need to provide your own configuration 
header. See the [examples from RTLib](https://github.com/Derppening/RTLib/tree/master/src/config) to learn how to write 
your own.

5. Create your `main.cpp`, and modify this template's `CMakeLists.txt`.

Just start off with something simple!

```cpp
int main() {}
```

6. Try to build the application.

```bash
# Create a new folder to house all CMake-related files.
mkdir cmake-build && cd cmake-build

# Change the -G option according to your build system, might be different for example if you were using MinGW.
cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_TOOLCHAIN_FILE=../RTLib/cmake/arm-toolchain.cmake -G "CodeBlocks - Unix Makefiles" ..

# Finally run the make command to initate the build process. This will build everything into the "cmake-build" folder.
cmake --build . --target all
```

The resulting binaries (`RTLib-Template_Debug.bin` and `RTLib-Template_Debug.elf`) will be located in the same 
directory.

If you are using CLion, you can skip all the terminal commands. However, you will have to specify the 
`CMAKE_TOOLCHAIN_FILE` variable manually in `Settings > Build, Execution, Deployment > CMake > CMake Options`.

### Testing on a Device

You should have either an ST-Link, USB-TTL, or JLink for your device. Check your mainboard if you are unsure.

All following code assumes your current working directory is where your binaries are.

#### ST-Link

```bash
../RTLib/scripts/flash_stlink.sh ../[target].bin  # Replace target.bin with the appropriate file
```

#### JLink

```bash
../RTLib/scripts/flash_jlink.sh <device> ./[target].bin  # Replace target.bin with the appropriate file
```


