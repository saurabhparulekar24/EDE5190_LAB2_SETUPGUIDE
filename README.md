# EDE5190_LAB2_SETUPGUIDE
Setup Guide for Running C codes on RP2040(QTpy Board)
This guide will help you to setup the WSL on your windows machine, then further help you to install the SDK on QTpy board and create compile and run a code written in C language
```
Saurabh Sandeep Parulekar
LinkedIn: https://www.linkedin.com/in/saurabh-parulekar/
Tested on: HP Pavillion Gaming Laptop, Windows 10 21H2, with WSL2
```
## WSL Setup Guide(Windows Linux Subsystem)
First you need to check whether your system is capable for the WSL
```
You must be running Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11.
```
`Note:` To check your Windows version and build number, select Windows logo key + R, type winver, select OK. You can update to the latest Windows version by selecting Start > Settings > Windows Update > Check for updates

Open windows Command prompt of Windows PowerShell in administrator mode and type the following command to install WSL
```
wsl --install
```
You will get the following message

![1 ](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/installation%20message.png)

Restart your computer, a command prompt will automatically start and you will be asked to enter a name for your ubuntu machine and a password. Once you do that you will see the following screen

![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/2.png)

Now in WSL USB devices won't be supported directly therefore you need install some extra drives to do so, First you need to install drivers on your windows machine
so go to this link([the usbipd-winproject](https://github.com/dorssel/usbipd-win/releases)) and Select the .msi file, which will download the installer. (You may get a warning asking you to confirm that you trust this download).Run the downloaded usbipd-win_x.msi installer file.

Now You need to install some tools on your linux machine, go the terminal in which your Ubuntu machine is running and enter the followinf commands
```
sudo apt install linux-tools-5.4.0-77-generic hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/5.4.0-77-generic/usbip 20
```
You will get a response as below when you enter the second command
![response](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/USB_Command.png)

This ends the installation of WSL

## Installing Visual Code Editor for editing Codes on WSL
THe WSL is a terminal based access, we do get any visual interface, hence to edit codes it becomes difficult, if you do not need a visual code editor you can use nano,vim etc. for now we'll install Visual Studio Code

Download Visual Studio Code from [Link](https://code.visualstudio.com/download)
after downloading we need to install some extensions, namely WSL and C/C++
Open Visual Studio code and click on the Extensions Option on the left vertical bar with icons

![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/VSC.png)

Search for WSL and C/C++ and click install
![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/VSC2.png)

This is the setup for Visual Studio Code, I'll show later in the guide on how to use it

## Getting Started with Installing the SDK on RP2040
Open your Ubuntu WSL terminal and type the below commands

```
$ cd ~/
$ mkdir pico
$ cd pico
```

Now we clone the SDK repositories into the above created directory, type the below commands to do so

```
$ git clone -b master https://github.com/raspberrypi/pico-sdk.git 
$ cd pico-sdk 
$ git submodule update --init 
$ cd .. $ git clone -b master https://github.com/raspberrypi/pico-examples.git
```

`Note:` NOTE: Failure to run the git submodule update --init command above will mean that the tinyusb module will not be included, and as a result USB functionality will not be compiled into the SDK. This means that USB serial, other USB functions, and example code will not work

To build the applications in pico-examples, you’ll need to install some extra tools. To build projects you’ll need CMake, a cross-platform tool used to build the software, and the GNU Embedded Toolchain for Arm. You can install both these via apt from the command line. Anything you already have installed will be ignored by apt.

```
$ sudo apt update 
$ sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential 





