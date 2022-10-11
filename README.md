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

Now, in WSL, USB devices won't be supported directly therefore you need install some extra drivers to do so, First you need to install drivers on your windows machine
so go to this link([the usbipd-winproject](https://github.com/dorssel/usbipd-win/releases)) and Select the .msi file, which will download the installer. (You may get a warning asking you to confirm that you trust this download).Run the downloaded usbipd-win_x.msi installer file.

Now You need to install some tools on your linux machine, go the terminal in which your Ubuntu machine is running and enter the following commands
```
sudo apt install linux-tools-5.4.0-77-generic hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/5.4.0-77-generic/usbip 20
```
You will get a response as below when you enter the second command
![response](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/USB_Command.png)

This ends the installation of WSL

## Installing Visual Code Editor for editing Codes on WSL
THe WSL is a terminal based access, we do get any visual interface, hence it becomes difficult to edit large codes, if you do not need a visual code editor you can use nano,vim etc. for now we'll install Visual Studio Code

Download Visual Studio Code from [Link](https://code.visualstudio.com/download)
after downloading we need to install some extensions, namely WSL and C/C++
Open Visual Studio code and click on the Extensions Option on the left vertical bar with icons

![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/VSC.png)

Search for WSL and C/C++ and click install
![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/VSC2.png)

This ends the setup for Visual Studio Code, I'll show later in the guide on how to use it

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
$ sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential libstdc++-arm-none-eabi-newlib
```

`Note:` NOTE: When a new version of the SDK is released you will need to update your copy of the SDK. To do this go into the pico-sdk directory which contains your copy of the SDK, and do the following

```
$ cd pico-sdk 
$ git pull 
$ git submodule update
```

## Compiling and Running the Code RP2040
Before we write a code we need to do a small setup

```
$ cd ~/
$ cd pico/pico-examples
$ mkdir build 
$ cd build
$ export PICO_SDK_PATH=~/pico/pico-sdk
$ cmake
```

You will get the following output
```
Using PICO_SDK_PATH from environment ('../../pico-sdk') 
PICO_SDK_PATH is /home/pi/pico/pico-sdk
  .
  .
  -- Build files have been written to: /home/pi/pico/pico-examples/build
```
Now to we will edit a code which is present in the pico-examples 

```
$ cd ~/
$ cd /pico/pico-examples/hello_world/usb
$ code .
```
The "code ." will open the directory in Visual Studio Code, where we can view and edit and save the file, we do not need to edit anything in the file as of now

We can see the hello_usb.c code

![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/hello_code.png)

We can also see that the other file in the directory, in that we can see that the serial output is routed to the USB instead of the UART

![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/cmake.png)

We are going to compile and run hello_usb.c code by following the steps

```
$ cd ~/
$ cd pico/pico-examples/build/hello_world
$ make -j4
```

Once this completes, we navigate into the hello_world folder and retrieve "hello_usb.uf2" file.
When we connect the Qtpy board it first connects to windows, we need to connect it to the virtual linux system, once we do that we face a problem.
to flash this code to the board, we need to put the board in programming mode, in doing so it gets disconnected from the linux machine and reconnects to windows as a Storage drive, when you connect it again to Linux, it does not appear as a storage drive which is necessary for flashing the code. Instead what we are gonna do is, we will copy the code from the linux machine to the windows machine and then flash the board.

```
$ cd ~/
$ cd pico/pico-examples/build/hello_world/usb
$ cp hello_usb.uf2 /mnt/c/Users/Saurabh/Downloads
```

The destination directory will change for you
```
$ cp hello_usb.uf2 /mnt/c/Users/<Your Machine Name>/Downloads
```

after the above step you'll find the code in your Downloads folder on windows machine


Connect your RP2040 board, while pressing the boot button press reset, RPI-RP2 drive will appear, once the drive appears, drag-drop the "hello_usb.uf2" file on that drive. the drive will disappear and your code will start running on the board

To view the output I used MobaXterm software [link](https://mobaxterm.mobatek.net/), it is a versatile software for various sessions such as SSH,telnet, serial etc.
Open MobaXterm and click on session on top left, select Serial and select the USB device(your RP2040 board, you can find the COM port in device manager) and select a baud rate of 115200

![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/Moba.png)

Press Ok and you will see the output of your code

![](https://github.com/saurabhparulekar24/EDE5190_LAB2_SETUPGUIDE/blob/main/mobaoutput.png)

Thank you






