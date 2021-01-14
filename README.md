# blue-pill-blinky-vscode

## Purpose

The purpose of this is demonstrate using CubeMX to generate code for the BluePill MCU board and to use VS Code
to build and debug the application. This is for Windows only. The sections below will walk through the entire proccess from setting up the toolchain to compiling and debugging. There are 3 sections:

* Prerequisites
* About
* Build and Debug

## Prerequisites

These are the prerequisites to in order to compile and flash the MCU. You must install all tools before continuing.

### OpenOCD

1. Download the latest version of [openocd](https://gnutoolchains.com/arm-eabi/openocd/)
and exract to "C:\embedded". If the directory doesnt already exist, create it.
2. Add "C:\embedded\\\<OpenOCD-Version>\bin" to the Path enviromental variable.
3. Run the command ```openocd --version``` in the terminal to confirm everything is working.

### ARM GCC

1. Download the latest version of [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads). We will call it by the binary name "arm-eabi-none-gcc".
2. Extract "C:\embedded". If the directory doesnt already exist, create it.
3. Add "C:\embedded\bin" to the Path enviromental variable.
4. Run the command ```arm-none-eabi-gcc --version``` in the terminal to confirm everything is working.

### VS Code Setup

1. Download and install [VS Code](https://code.visualstudio.com/download).
2. Once installed, click on plugins icon (looks like some blocks on the right).
3. Install the "C/C++" plugin.
4. Install the "Cortex-Debug" plugin. This plugin is how you will debug your MCU.

### Git

1. Download and install [git](https://git-scm.com/downloads).
2. Clone the repo using the following: ```git clone https://github.com/Nick-Zuchlewski/blue-pill-blinky-vscode.git```
3. Open VS Code and click on "file > open folder". Find where you cloned the repo and open that folder.

### Make

There are a few ways to install make. This will focus on the powershell.
*Note: I am having problems in WSL2 Debian with this. Make works but gcc cannot be found. I suspect this is becuase I exported the path in ~.zshrc and did not symbolically linky the binaries. It looks like VS code also uses /bin/sh with WSL even when the defualt shell is zsh.*

1. Open Powershell in Administrative mode.
2. Install [choco](https://chocolatey.org/courses/installation/installing?method=installing-chocolatey#powershell)
3. Use choco to install [make](https://chocolatey.org/packages/make)
4. If VS Code was open, close and re-open it.

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
* Include Path is set to recursively searh for all headers. Sometimes VS Code will fail to find those headers. You may need to explicitly add them. If so, copy the "C_Includes" into the "IncludePath" and strip out the "-D". You may need to re-load VS Code sometimes.

#### [launch](https://code.visualstudio.com/docs/editor/debugging)

This file is used for debugging. It basically instructs "Cortex-Debug" how debug your application.

* Uses the elf artifact to flash the MCU.
* Points to the SVD that will map to the registers while in debug.
* Uses OpenOCD and the config files to point the ST-Link V2 and the corrct chip family.

## Build and Debug

### Build

When STM32CubeMX generates the code, it will generate a makefile (when specifying the IDE toolchain). You can run the this makfile like you normally would with ```make```. However, a build task is provided in the ".vscode/tasks.json". To build click "Terminal > Run Task" and select "build".

### Debug

You can debug by pressing "F5" or click "Run > Start Debugging". Ensure the MCU is connected before starting. Once started, the application will always pause at the begining of the assemnbly in the startup file. Press "F5" or the Continue button. You can add breakpoints at any time. To see more debug info, click on the Debug button on the left (looks like a bug on a play button). From here you can see your variables and registers. *Note: if you change the source, you will need to build again before debugging.*
