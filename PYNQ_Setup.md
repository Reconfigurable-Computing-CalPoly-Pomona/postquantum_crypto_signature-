# Running LMS on PYNQ_Z1 board

Video Walkthrough

## PYNQ Introduction
PYNQ is an open-source project by Xilinx that allows the uage of python for rapid protoyping. The chipset that the board uses is the Zynq-7000 series. This chip contains both a dual-core ARM Cortex-A9 processor and fpga fabric. This gives the flexibility to run an arm based operating system in combination with a custom fpga hardware design. The board this project currently supports and was created for is the PYNQ-Z1

## LMS Introduction

## Requirements
* [PYNQ-Z1 board](http://www.pynq.io/board.html)
* USB to Micro-USB cable
* Micro SD card 8Gb or more
* Serial Port Terminal (GTKterm, Putty, TeraTerm, etc.)
* Ethernet cable (Optional)
* Software for flashing (Etcher, Rufus, etc.)

## Setup

Steps taken to use custom image in order to run/test the lms code on the PYNQ board

### Creating Bootable SD Card
Bootable images running ubuntu arm with LMS software preinstalled:
[PYNQ-Z1](https://drive.google.com/file/d/1cGJpK71YlWuMF9Sf-PXz1tq_aS4WoBCR/view?usp=sharing)

Once the image has finished downloading the next step is to flash the image onto the sd card. Take the sd card and plug it into the computer. Make sure it has at least 8Gb and has been formatted (Contains no files or info on it). If there are any files on the sd card they will be lost. The following pictures shows what it would look like in [Etcher](https://www.balena.io/etcher/)
![Flashing Setup](/Pictures/Etcher_Flashing.png)

![Finished Flashing](/Pictures/Etcher_Flashed.png)

Once flashing has completed the partitions should look like the following picture. This step is optional to check if flashing succeeded.
![](/Pictures/Card_Partitions.png)

### Running the image on the PYNQ

## Running LMS

## Additional Resources

### LMS

### PYNQ
[PYNQ Website](http://www.pynq.io/)

