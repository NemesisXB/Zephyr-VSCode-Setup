# Zephyr-VSCode-Setup
Setup guide for Zephyr in VS Code

## Links
[Debug in VS Code (bus710)](https://github.com/bus710/zephyr-rtos-development-in-linux)\
[Debug in VS Code (Simon Wijk)](https://gitlab.endian.se/simon/vscode-zephyr-intro)\
[Ubuntu Update Device Tree Compiler(DTC)](https://lists.zephyrproject.org/g/users/topic/dtc_version_unsupported_error/32325016?p=,,,20,0,0,0::recentpostdate%2Fsticky,,,20,2,0,32325016)\
[Fix OpenOCD gdb-attach Error](https://mcuoneclipse.com/2016/04/09/solution-for-openocd-cannot-communicate-target-not-haltet/)

## Example vscode files
launch.json
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Zephyr STM32F401",
            "cwd": "${workspaceRoot}",
            "executable": "build/zephyr/zephyr.elf",
            "request": "launch",
            "type": "cortex-debug",
            "servertype": "openocd",
            "device": "STM32F401RE",
            "interface" : "swd",
            "armToolchainPath": "${HOME}/zephyr-sdk/arm-zephyr-eabi/bin",
            "gdbpath": "/usr/bin/gdb-multiarch",
            "configFiles": [
                 "/usr/share/openocd/scripts/board/st_nucleo_f4.cfg"
             ],
             "preLaunchTask": "Build"
        }
    ]

```

tasks.json
```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Build",
            "type": "shell",
            "command": "west",
            "args": [
                "build",
                "-b nucleo_f401re",
                "--force"
            ],
            "options": 
            {
                "cwd": "${workspaceFolder}",
                "env": {
                    "ZEPHYR_TOOLCHAIN_VARIANT": "zephyr",
                    "ZEPHYR_SDK_INSTALL_DIR": "${env:HOME}/zephyr-sdk"
                }
            },
            "problemMatcher": {
                "base": "$gcc",
                "fileLocation": [
                    "absolute"
                ]
            },
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "Build Verbose",
            "type": "shell",
            "command": "west",
            "args": [
                "-v",
                "build",
                "-b nucleo_f401re",
                "--force",
                "--",
                "-DCMAKE_EXPORT_COMPILE_COMMANDS=ON"
            ],
            "options": 
            {
                "cwd": "${workspaceFolder}",
                "env": {
                    "ZEPHYR_TOOLCHAIN_VARIANT": "zephyr",
                    "ZEPHYR_SDK_INSTALL_DIR": "${env:HOME}/zephyr-sdk"
                }
            },
            "problemMatcher": {
                "base": "$gcc",
                "fileLocation": [
                    "absolute"
                ]
            },
            "group": "build"
        },
        {
            "label": "Clean",
            "type": "shell",
            "command": "west",
            "args": [
                "build",
                "-b nucleo_f401re",
                "-t",
                "clean",
                "--force"
            ],
            "options": 
            {
                "cwd": "${workspaceFolder}",
                "env": {
                    "ZEPHYR_TOOLCHAIN_VARIANT": "zephyr",
                    "ZEPHYR_SDK_INSTALL_DIR": "${env:HOME}/zephyr-sdk"
                }
            },
            "group": "build"
        },
        {
            "label": "Flash",
            "dependsOn": [
				"Build"
			],
            "type": "shell",
            "command": "west",
            "args": [
                "flash"
            ],
            "options": 
            {
                "cwd": "${workspaceFolder}",
                "env": {
                    "ZEPHYR_TOOLCHAIN_VARIANT": "zephyr",
                    "ZEPHYR_SDK_INSTALL_DIR": "${env:HOME}/zephyr-sdk"
                }
            },
            "problemMatcher": {
                "base": "$gcc",
                "fileLocation": [
                    "absolute"
                ]
            },
            "group": "build"
        },
    ]
}

```

c_cpp_properties.json
```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [],
            "compilerPath": "${HOME}/zephyr-sdk/arm-zephyr-eabi/bin/arm-zephyr-eabi-gcc",
            "cStandard": "c99",
            "cppStandard": "c++17",
            "intelliSenseMode": "${default}",
            "compileCommands": "${workspaceFolder}/build/compile_commands.json"
        }
    ],
    "version": 4
}

```
## Add tasks to VS Code Taskbar
Install VS Code plugin: **actboy168.tasks**:\
Name: Tasks\
Id: actboy168.tasks\
Description: Load VSCode Tasks into Status Bar.\
Version: 0.3.6\
Publisher: actboy168\
[VS Marketplace Link](https://marketplace.visualstudio.com/items?itemName=actboy168.tasks)
