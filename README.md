# SKR_Mini_E3_V3_DFU
Code and Jumper to Implement DFU Mode Programming on a BTT SKR Mini E3 V3
![SKR Mini E3 V3 with DFU mode jumper in place.](https://github.com/3dApothecary-xyz/SKR_Mini_E3_V3_DFU/blob/main/Images/2025.04.26-SKR_Mini_E3_V3-Landmarks-Clipped.png)
The use of "Micro" SD Cards (also known as "TF Cards") to upload 3d printer firmware into the controller's microcontroller (MCU) goes back to the first days of 3D printing but it is a a process that requires direct access to the controller as well as having an appropriately set up SD card for the MCU's bootloader along with adapter for loading in new firmware.  The STMicro "DFU Mode" is a much more convenient method to load firmware onto an STM32 MCU as it does not require a Micro SD Card or a bootloader - instead, it uses a Flashing tool built into the STM32 MCU that allows firmware to be written to the device's Flash directly through USB using [dfu-util](https://dfu-util.sourceforge.net/) which can be run from a PC (Windows and Linux), a Mac or a Raspberry Pi or similar Small Board Computer (SBC).  

The [BTT SKR Mini E3 V3](https://biqu.equipment/products/bigtreetech-skr-mini-e3-v2-0-32-bit-control-board-for-ender-3) is a great controller board for low-end 3D printers.  It is a attractive, well laid out and well equipped with four TMC2209 stepper motor drivers built in making it an ideal upgrade to a basic 3D printer or used in a custom printer with the only downside being the need to load new firmware onto a Micro SD Card which can be cumbersome.  The code presented here will change the "Option Bytes" of the STM32G0B1 MCU built into the SKR Mini E3 V3, allowing it to run in DFU Mode.  

## WARNING

The executable code presented here was created **ONLY** for the STM32G0B1 and has **ONLY** been tested on the SKR Mini E3 V3.  Using it on any other board is ****NOT RECOMMENDED**** and could end up bricking the board and the MCU built into it.  

## NOTE

Most pre-packaged firmware is built to run at a set address after the bootloader.  When using DFU Mode, ensure that you understand where the firmware is expected to be loaded and set this into the dfu-util address when Flashing the firmware into the BTT SKR Mini E3 V3.  

Using DFU Mode, the bootloader that comes with the BTT SKR Mini can be overwritten with new firmware but, again, the execution address the firmware is built for must be understood and match the start of the Flash in the MCU (0x08000000 in the STM32G0B1).  

If you are using this application to replace the bootloader with Katapult (the Klipper installation program) make sure that Katapult is bulit with **NO** offset specified.  

## Equipment Required

The following pieces of kit are required to enable DFU Mode on a BTT SKR Mini E3 V3:

1. BTT SKR Mini E3 V3 with jumper (included with the controller board)

3. PC/Mac/SBC loaded with [dfu-util](https://dfu-util.sourceforge.net/) and equipped with a Micro SD Card adapter

4. Micro SD Card - ideally no larger than 8GB

5. Micro USB Cable to connect the PC/Mac/SBC to the BTT SKR Mini E3 V3

![Jumper Block](https://github.com/3dApothecary-xyz/SKR_Mini_E3_V3_DFU/blob/main/Images/2025.04.26-SKR_Mini_E3_V3-DFU-Jumper-Resized.png)

6. Jumper Block or a jumper wire.  The Jumper Block shown above consists of a 5x1 Dupont connector with the first and fourth positions tied together as can be seen above.  The white painted stripe on the Jumper Block is to indicate Pin 1 and how it is to be installed on the BTT SKR Mini E3 V3

7. White Paint Marker.  While optional, it is useful in marking Pin 1 on the BTT SKR Mini E3 V3 

## Equipment Preparation

If following exactly to the example, then:

1. Mark Pin 1 of the SWD Header on the BTT SKR Mini E3 V3 board using the White Paint Marker.  This can be seen in the yellow oval that is inside the red circle
   
2. Crimp Dupont female connectors at the end of a short wire and insert into pins one and four of a 5x1 Dupont header.  It is recommended that Pin 1 of the 5x1 Dupont Header is marked with paint as shown in the image above

## Code Install Process

1. Install [dfu-util](https://dfu-util.sourceforge.net/) into the system that will be used to program the BTT SKR Mini E3 V3.  Note that Klipper installations on SBCs using KIAUH, [dfu-util](https://dfu-util.sourceforge.net/) is loaded in with other required utilities

2. Format Micro SD Card.  For the example here, a 4GB Micro SD Card formwatted with "FAT32" in full mmode (not "Quick Format")

3. Copy [MINI_E3_V3_DFU.bin](https://github.com/3dApothecary-xyz/SKR_Mini_E3_V3_DFU/blob/main/Executable/MINI_E3_V3_DFU.bin) and rename it "firmware.bin"

![USB Jumper Block](https://github.com/3dApothecary-xyz/SKR_Mini_E3_V3_DFU/blob/main/Images/2025.04.26-USB-Jumper-Marked.png)

4. Install the jumper on the two pin "SW_USB" header on the BTT SKR Mini E3 V3 as shown in the red oval put on the image above.  This will allow the programming system to power the BTT SKR Mini E3 V3.  **NOTE:** This will have to be removed when the BTT SKR Mini E3 V3 is installed in the 3D printer

5. Insert the Micro SD Card into the BTT SKR Mini E3 V3's SD Card Port

6. Plug the BTT SKR Mini E3 V3 into the system that will be used to program it using the Micro USB Cable

7. After a few seconds, the "STATUS" LED (beside the power LED on the left hand side of the BTT SKR Mini E3 V3) will start flashing (three short and one long) to indicate that Optoin Bytes are set correctly to enable DFU Mode

8. Unplug the BTT SKR Mini E3 V3 from the programming system

9. Remove the Micro SD Card from the BTT SKR Mini E3 V3 and insert it into the programming system

10. Check to see that the filename on the Micro SD Card has changed from "firmware.bin" to "firmware.cur".  This change indicates that the code was loaded into the MCU without issue

## Enabling DFU Mode and Loading new Firmware

1. Insert the jumper block's pin one into the SWD header of the BTT SKR Mini E3 V3 as shown in the red circle in the image at the top of this page

2. If testing DFU Mode without first installing the BTT SKR Mini E3 V3 into a 3D printer, reinsert the jumper into the "SW_USB" header as noted in point 4. of the previous section ("Code Install Process")

3. Connect the BTT SKR Mini E3 V3 into the programming system using the Micro USB Cable

4. If the BTT SKR Mini E3 V3 is installed in a 3D printer, turn on printer power

![Linux DFU Mode Check](https://github.com/3dApothecary-xyz/SKR_Mini_E3_V3_DFU/blob/main/Images/2025.04.26-lsub_Results.png)

5. If running a Linux system, then use the command `lsusb` to verify that the BTT SKR Mini E3 V3 is in DFU Mode and look for the device at `0483:df11` as shown in the image above

![Linux DFU Mode Check](https://github.com/3dApothecary-xyz/SKR_Mini_E3_V3_DFU/blob/main/Images/2025.04.26-SKR_Mini_E3_V3-DFU-Windows-Marked.png)

6. If running a Windows System, then use Device Manager to verify that the BTT SKR Mini E3 V3 is in DFU Mode as shown in the image above

7. With DFU Mode confirmed, install the new firmware using the [dfu-util](https://dfu-util.sourceforge.net/) command

8. When firmware installation is complete, remove the jumper block and press the Reset button (Green Oval in the top image) to start the new firmware

## Source Code

This application was created in STM32CUBEIDE as an STM32G0B1 application with PD8 (STATUS LED) specified as a Digital Output.  The modified files can be found in the [Source Folder](https://github.com/3dApothecary-xyz/SKR_Mini_E3_V3_DFU/tree/main/Source) on this GitHub page.  

The source files provided here can be used as the basis for code enabling DFU mode in other STM32 MCUs ****BUT**** consulting STMicro AN2606 and any other device specific documents is required to understand if and how DFU Mode can be enable for the specific MCU being used.  
