# blue-pill-blinky-vscode

## Purpose

The purpose of this is demonstrate using CubeMX to generate code for the BluePill MCU board and to use VS Code
to build and debug the application. This is for Windows only. The sections below will walk through the entire proccess from setting up the toolchain to compiling and debugging. There are 3 sections:

* Prerequisites
* About
* Build and Debug

---

## Prerequisites

These are the prerequisites to in order to compile and flash the MCU. You must install all tools before continuing.

### OpenOCD

1. Download the latest version of [openocd](https://gnutoolchains.com/arm-eabi/openocd/)
and exract to "C:\embedded". If the directory doesnt already exist, create it.
2. Add "C:\embedded\<OpenOCD-Version>\bin" to the Path enviromental variable.
3. Run the command ```openocd --version``` in the terminal to confirm everything is working.

### ARM GCC

1. [stuff](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)
2. Add
3. Run the command ```arm-none-eabi-gcc --version``` in the terminal to confirm everything is working.

### VS Code Setup

1. Download and install [VS Code](https://code.visualstudio.com/download).
2. Once uinstalled, click on plugins icon (looks like some blocks on the right).
3. Install the "C/C++" plugin.
4. Install the "Cortex-Debug" plugin. This plugin is how you will debug your MCU.

### Git (git gud)

1. Download and install [git](https://git-scm.com/downloads).
2. Clone the repo using the following: ```git clone https://github.com/Nick-Zuchlewski/blue-pill-blinky-vscode.git```
3. Open VS Code and click on "file > open folder". Find where you cloned the repo and open that folder.

---

## About

This section provides some key info about whats in the repo.

### SVD (System View Description)

Although the SVD is already provided in the repo, you can download them from [ST](https://www.st.com/en/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus.html#cad-resources).
The SVD is used by the "Cortex-Debug" plugin to get registor information.

### Code Generation

The code is already generate by [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html). The ioc file is located in the root directory.

### VS Code JSON Configuration

There is a .vscode directory. This directory cotains JSON files that are used by IDE to assist with debugging, IntelliSense, etc.

#### [c_cpp_properties](https://code.visualstudio.com/docs/cpp/c-cpp-properties-schema-reference)

This file configures the compiler and IntelliSense. This is the magic of VS code. Some key notes:

* The last few entries of the define are important and are taken from the makefile.
* The compiler is arm-eabi-none-gcc and the args are also taken from the makefile. The F1 is an M3 Cortex.

#### [launch](https://code.visualstudio.com/docs/editor/debugging)

This file is used for debugging. It basically instructs "Cortex-Debug" how debug your application.

* Uses the elf artifact to flash the MCU.
* Points to the SVD that will map to the registers while in debug.
* Uses OpenOCD and the config files to point the ST-Link V2 and the corrct chip family.

---

## Build and Debug
