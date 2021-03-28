# Running LMS on PYNQ_Z1 board

[Linux Tutorial With SSH](https://youtu.be/cnxKVU59BXg)

[Windows Tutorial With Serial Terminal](https://youtu.be/AtVcn33WBqA)

## PYNQ Introduction
PYNQ is an open-source project by Xilinx that allows the uage of python for rapid protoyping. The chipset that the board uses is the Zynq-7000 series. This chip contains both a dual-core ARM Cortex-A9 processor and fpga fabric. This gives the flexibility to run an arm based operating system in combination with a custom fpga hardware design. The board this project currently supports and was created for is the PYNQ-Z1

## LMS Introduction
Leighton-Micali Signatures (LMS) is a post-quantum robust hash-based signature scheme which allows for key generation , signature generation, and signature verification. LMS's structure is based on a binary Merkle Tree in which each node contains the hash of its two leaves. The lowest level leaves contain the Winternitz One-Time Signature (WOTS) chain. The hash function used in LMS by default is SHA-2/256. Due to the nature of LMS using WOTS, each signature generated with LMS is only a one time use. The variant of LMS used in this implementation is called Heirachial Signature System (HSS) and utilizes 2 separate levels of the binary Merkle Tree, each with a different height. This allows for increased performance because only parts of the tree need be generated at a given time. 

[LMS/HSS by Cisco](https://github.com/cisco/hash-sigs)

## Requirements
* [PYNQ-Z1 board](http://www.pynq.io/board.html)
* [USB to Micro-USB cable](https://www.amazon.com/UGREEN-Braided-Charger-Charging-Controller/dp/B01NBHYAR0/ref=sr_1_2?dchild=1&keywords=usb-A+to+micro+usb+cable&qid=1616939596&sr=8-2)
* [Micro SD card 8Gb or more](https://www.amazon.com/SanDisk%C2%AE-microSDHCTM-8GB-Memory-Card/dp/B0012Y2LLE/ref=sr_1_3?dchild=1&keywords=micro+sd+card+8Gb&qid=1616939669&sr=8-3)
* Serial Port Terminal (GTKterm, [Putty](https://www.putty.org/), [Tera Term](https://ttssh2.osdn.jp/index.html.en), etc.)
* [Ethernet cable (Optional)](https://www.amazon.com/AmazonBasics-RJ45-Cat-6-Ethernet-Patch-Cable-10-Feet-3-Meters/dp/B00N2VISLW/ref=sr_1_2?dchild=1&keywords=Ethernet%2Bcable&qid=1616939698&sr=8-2&th=1)
* Software for flashing ([Etcher](https://www.balena.io/etcher/), [Rufus](https://rufus.ie/), etc.)

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

### Connecting to the board via a serial terminal
#### Linux
To check what serial port is connected, open up the terminal (Ubuntu ```ctrl+alt+T```) and type in the following command
* ```sudo dmesg | grep tty```

The following image shows a sample output. Because the ARM processor is dual core each core gets its own serial port which should show up as ttyUSBx, where x is the number of connected serial ports. The latter port is the one that the serial terminal will communicate. In this case it is ```ttyUSB1```

![](/Pictures/Dmesg_Out.png)

In the serial terminal once the serial port has been selected the baud rate must be set to ```115200``` for proper communication.

![](/Pictures/Terminal_Config.png)

#### Windows
To find out the connected serial port in windows the device manager must be opened and the "Ports" section expanded. In this case the device is ```COM4```.

![](/Pictures/Windows_COM.PNG)

In the serial terminal the setup is similar to Linux. the baud rate must be set to ```115200``` and the correct COM port selected.

![](/Pictures/Windows_Config.PNG)

### Connecting to the board via ssh (for remote access)
First the board has to be connected to a router via an ethernet connection. Once that is connected there should be one or two extra LEDs that light up. This example will show how to set it up in linux. In Windows both Putty and Tera Term support ssh as well.

After the hardware is connected a terminal needs to be opened up. The ip address of the pynq must be known before using ssh. The default address is ```192.168.1.87```. Then enter the following command followed by the password of the device (```temppwd```). Once connected move onto the next section
* ```ssh ubuntu@192.168.1.87```

![](/Pictures/Ssh.png)

## Running LMS
It is important to note that this demo can run on a single device to test its capabilities.

Once the PYNQ is connected and turned on, the serial port must be opened from the terminal being used. The image will initialize and it may take a few seconds until it finishes and waits for an input. The username is ```ubuntu``` and the password is ```temppwd```.

![](/Pictures/Boot_Image.png)

First the directory must be moved which can be accomplished by the command:
* ```cd hash-sigs/```

After that a message that will be signed must be created. For example here the message created is called ```message.txt```, which can be created by running 
* ```nano message.txt```

![](/Pictures/Message.png)

To see what operations the demo can run just type the command ```./demo``` and all possibilities will be displayed

![](/Pictures/Demo.png)

The next step is to generate the key by running the following command. The generation currently will take approximately 3 hours. Here the "pynqKey" is just the name for the key selected but it can be changed to any name.
* ```./demo genkey pynqKey```

![](/Pictures/Genkey_Start.png)
![](/Pictures/Genkey_End.png)

Once the key has been created it must now be signed by typing in the command;
* ```./demo sign pynqKey message.txt```

![](/Pictures/Demo_Sign.png)

After signing it can be verified as well. The output will report back whether the data is valid or invalid, meaning it has been tampered with or is incorrect.
* ```./demo verify pynqKey message.txt```

![](/Pictures/Demo_Valid.png)
![](/Pictures/Demo_Invalid.png)

## Additional Resources

### LMS
[LMS Website](https://csrc.nist.gov/Projects/stateful-hash-based-signatures) 

### PYNQ
[PYNQ Website](http://www.pynq.io/)


