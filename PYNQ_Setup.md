# Running LMS on PYNQ_Z1 board

Video Walkthrough

## PYNQ Introduction
PYNQ is an open-source project by Xilinx that allows the uage of python for rapid protoyping. The chipset that the board uses is the Zynq-7000 series. This chip contains both a dual-core ARM Cortex-A9 processor and fpga fabric. This gives the flexibility to run an arm based operating system in combination with a custom fpga hardware design. The board this project currently supports and was created for is the PYNQ-Z1

## LMS Introduction

## Requirements
* [PYNQ-Z1 board](http://www.pynq.io/board.html)
* USB to Micro-USB cable
* Micro SD card 8Gb or more
* Serial Port Terminal (GTKterm, Putty, Tera Term, etc.)
* Ethernet cable (Optional)
* Software for flashing (Etcher, Rufus, etc.)

## Setup

Steps taken to use custom image in order to run/test the lms code on the PYNQ board

### Creating Bootable SD Card
Bootable images running ubuntu arm with LMS software preinstalled:
[PYNQ-Z1](https://drive.google.com/file/d/1cGJpK71YlWuMF9Sf-PXz1tq_aS4WoBCR/view?usp=sharing)

Once the image has finished downloading the next step is to flash the image onto the sd card. Take the sd card and plug it into the computer. Make sure it has at least 8Gb and has been formatted (Contains no files or info on it). If there are any files on the sd card they will be lost. The following pictures shows what it would look like in [Etcher](https://www.balena.io/etcher/)

![](/Pictures/Etcher_Flashing.png)
![](/Pictures/Etcher_Flashed.png)

Once flashing has completed the partitions should look like the following picture, with one boot and one root paritition. This step is optional to check if flashing succeeded.

![](/Pictures/Card_Partitions.png)

Next the PYNQ must be [configured](https://pynq.readthedocs.io/en/latest/getting_started/pynq_z1_setup.html) so it can properly boot from an sd card which should be in the same position as shown in 1. The sd card is inserted on the bottom of the board as shown in 3.

![](/Pictures/Board_Setup.PNG)

### Connecting to the board via a serial Terminal
### Linux
To check what serial port is connected, go to the terminal and type in the following command
* ```sudo dmesg | grep tty```

The following image shows a sample output. Because the ARM processor is dual core each core gets its own serial port which should show up as ttyUSBx, where x is the number of connected serial ports. The latter port is the one that the serial terminal will communicate. In this case it is ```ttyUSB1```

![](/Pictures/Dmesg_Out.png)

In the serial terminal once the serial port has been selected the baud rate must be set to ```115200``` for proper communication.

![](/Pictures/Terminal_Config.png)

### Windows
To find out the connected serial port in windows the device manager must be opened and the "Ports" section expanded. In this case the device is ```COM4```.

![](/Pictures/Windows_COM.PNG)

In the serial terminal the setup is similar to Linux. the baud rate must be set to ```115200``` and the correct COM port selected.

![](/Pictures/Windows_Config.PNG)

## Running LMS
It is important to note that this demo can run on a single device to test its capabilities.

Once the PYNQ is connected and turned on, the serial port must be opened from the terminal being used. The image will initialize and it may take a few seconds until it finishes and waits for an input. The username is ```ubuntu``` and the password is ```temppwd```.

![](/Pictures/Boot_Image.png)

First the directory must be moved which can be accomplished by the command:
* ```cd hash-sigs/```

After that a message that will be signed must be created. For example here the message created is called ```message.txt```.

![](/Pictures/Message.png)

To see what operations the demo can run just type the command ```./demo``` and all possibilities will be displayed

![](/Pictures/Demo.png)

The next step is to generate the key by running the following command. The generation currently will take approximately 3 hours. Here the "pynqKey" is just the name for the key selected but it can be changed to any name.
* ```./demo genkey pynqKey```

Once the key has been created it must now be signed by typing in the command;
```TO DO```

![](/Pictures/Demo_Sign.png)

After signing it can be verified as well. The output will report back whether the data is valid or invalid, meaning it has been tampered with or is incorrect.
```TO DO```

![](/Pictures/Demo_Valid.png)
![](/Pictures/Demo_Invalid.png)

## Additional Resources

### LMS

### PYNQ
[PYNQ Website](http://www.pynq.io/)

