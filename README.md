# Sens'it SDK

The Sens'it is a multi-sensor device that uses the Sigfox Network to communicate.  
It embeds a button, two LEDs, a temperature & relative humidity sensor, an ambient light sensor, an accelerometer, a magnetometer and a reed switch.

The Sens'it SDK makes it possible for you to change the behavior of the device to match your needs.

## Getting Started

### Download the SDK

Go to [Sigfox Build](https://build.sigfox.com/sensit-for-developers#reprogram-the-sensit-firmware-with-the-sdk) to download the full Sens'it SDK. The download link as at the bottom of the section.
**Note:** The GitHub repository does not contain the SDK library (required to build your firmware) & binary files.

### Software Installation

To use the SDK, please install the following software:

- [GNU Arm Embedded Toolchain 7-2017-q4-major](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)

- [dfu-util 0.9](http://dfu-util.sourceforge.net/)

### Sens'it 2 Upgrade

The Sens'it SDK was created with the the Sens'it 3 as its main target.  
**If you use a Sens'it 2, follow the steps below to upgrade your device and make it compatible with the SDK.**

1. Set your Sens'it 2 on Standby/Button mode: keep the button pressed until the main LED is off and the secondary LED is on.
2. Connect Sens'it to your computer.
3. Do 4 short presses on the button.
4. When the secondary LED starts blinking, do a long press on the button.
5. If both LEDs become white, your device is in bootloader mode; otherwise go back to step 3.
6. Run the following command to do a backup of your device memory in case of an upgrade failure:
```
dfu-util -a 0 -s 0x08000000:32768 -U backup.bin
```
7. Program your Sens'it with the firmware `upgrade.bin` using the following command:
```
dfu-util -a 0 -s 0x08000000:leave -D bin/upgrade.bin
```
8. If the main LED is:
    - **BLUE**, your device was already compatible with the SDK and the upgrade was not needed.
    - **GREEN**, the upgrade was successful.
    - **YELLOW**, the upgrade has failed.
    - **RED**, your device cannot be upgraded.
9. To put your device back in bootloader mode, redo step 3 to 5.
10. If the upgrade was successful you can now program your device with your custom firmware. If not, run the following command to restore your device to its initial state:
```
dfu-util -a 0 -s 0x08000000:leave -D backup.bin
```

## Develop your firmware

Many functions are available to help you develop your custom application:
 - [Sens'it APIs](sdk/inc/sensit/sensit_api.h)
 - [Sigfox Radio](sdk/inc/radio/radio_api.h)
 - [Temperature sensor](sdk/inc/sensors/hts221.h)
 - [Ambient light sensor](sdk/inc/sensors/ltr329.h)
 - [Accelerometer](sdk/inc/sensors/fxos8700.h)

The API documentation is available in the full SDK archive, in the /doc/html/modules.html file.  
You can also consult the [F.A.Q.](doc/FAQ.md) section, and the list of [reserved prefixes](doc/reserved-prefix.md) that you should not use for your functions.

To start your development, use the [main](sdk/src/main.c) template.

### Sample codes

Here is the list of available sample codes that may help you:

| Use case                                    | Doc                        |
|---------------------------------------------|----------------------------|
| Temperature & relative humidity measurement | [here](doc/temperature.md) |
| Ambient light measurement                   | [here](doc/light.md)       |
| Movement detection                          | [here](doc/vibration.md)   |
| Magnet detection                            | [here](doc/magnet.md)      |
| AT commands parser                          | [here](doc/modem.md)       |

## How to Build

A Makefile is provided to ease the building of all sources.  
You can edit the following fields to match your need:
 - **TOOL_PATH**: to add the path to the toolchain.
 - **OUTPUT_NAME**: to change the name of the firmware binary.
 - **CC_INC**: to add new headers directories.
 - **VPATH**: to add new sources directories.
 - **OBJ_LIST**: to add new object targets to build.
 - **CC_FLAGS**: to add some compilation flags.

By default, the firmware will use the Radio Configuration (RC) set in your device.  
If you need another RC, use one of these flags: `-DUSE_RC1`, `-DUSE_RC2`, `-DUSE_RC3`, or `-DUSE_RC4`.  
Then, use the command `make` to build your firmware.

The size of a Sens'it firmware can be up to 29 kB.  
**Do not modify the Linker Script to increase the firmware size, otherwise you may erase critical data from the device.**

## Program your Sens'it

To program your Sens'it you will need to put it in bootloader mode:
1. Connect your device to your computer.
2. Reset your device. With one of the provided firmwares, you can do this with 4 short presses on the button.
3. When the secondary LED starts blinking, do a long press on the button.
4. If both LEDs become white, your device is in bootloader mode.

Then, use the `make prog` command to program your Sens'it.

If you want to return to the original firmware of the Sens'it 3, use the following command to program it:
```
dfu-util -a 0 -s 0x08000000:leave -D bin/sensit_discovery_vX.X.X.bin
```
Replace `X.X.X` with the current version of the Discovery firmware available in the `bin` folder of the full SDK archive.

## Version

- v2.0.0

## Authors

- Sens'it Team

## Copyright

- Copyright (c) 2018 Sigfox, All Rights Reserved.
