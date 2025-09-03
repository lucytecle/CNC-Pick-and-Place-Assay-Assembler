# CNC Arduino Script

## Description
This repository includes code for controlling a Vevor CNC 3040 using an Arduino. In addition, the drillbit was replaced by a syringe connected to a vacuum pump; together, the functions in this repository allow the CNC
machine to act as a pick-and-place. This project was designed for automated assay test assembly using paper-based components. 

## Quick Start
1. Install [Arduino IDE](https://www.arduino.cc/en/software).  
2. Connect the Arduino Uno that is configured for the CNC workstation. 
3. Open the `.ino` file from the **/Software/** folder for LFIA control, or from **/Supplemental/** for dipstick/duplex control.  
4. Compile and upload the code to the Arduino.  
5. Refer to the HardwareX manuscript (linked below) for full operation instructions.  

## Repository Structure
- **/Assay Holders/** → STL files for 3D-printed assay holders (dipstick, LFIA, duplex).  
- **/Software/** → Arduino firmware (`.ino`, `.h`) for LFIA assembly.  
- **/Supplemental/** → Arduino firmware for alternative assay formats (dipstick, duplex).  
- **README.md** → Documentation file (this document).  

## Background
The CNC machine is typically controlled by a computer software (LaserGRBL) which runs G-code. However, it was recognized that the CNC could also be controlled by an offline joystick using UART 8-bit wordsize, no parity,
1-bit stop @ 115.2kBaud. This functionality was utilized to allow the CNC machine to be controlled by an Arduino. The offline controller was found to have the following pinout:
1 3 5 7
2 4 6 8
Where:
Pin 1 - No Connect
Pin 2 - UART_RX
Pin 3 - Reset
Pin 4 - UART_TX
Pin 5 - GND
Pin 6 - GND
Pin 7 - PWR
Pin 8 - PWR

**Connections:**  
- Pin 2 of the CNC → IO1 of Arduino  
- Pin 6 of the CNC → GND  
- Pin 8 of the CNC → +5V  
- Remaining pins left floating  
Note: If IO0 is connected to Pin 4 of the CNC, the Arduino will NOT be able to be reprogrammed!**

**Firmware notes:**  
- Works by sending GRBL commands from the Arduino to the CNC.  
- GRBL syntax documentation: [GRBL Commands](https://github.com/gnea/grbl/blob/master/doc/markdown/commands.md).  
- Most commands require a line-feed character (`\n`) to mark the end of the command.  

**Vacuum pump integration:**  
- A 400147-142-D solenoid connected to a vacuum pump was controlled by the Arduino using a relay shield.  
- The solenoid functions at 24-30V and requires ~300 mA.  

## Functions:
- **void absoluteMove(float x, float y, float z)** takes input Cartesian coordinates (in millimeters) and moves the CNC to that position.
- **void grab(void)** turns on the vacuum pump and lowers the syringe to pick up a component. The syringe is then raised.
- **void release(void)** lowers the syringe, turns off the vacuum pump, waits for the component to release, and then raises the syringe.

## License
- **Code:** Licensed under the MIT License (see license text in file headers).  
- **Repository (data package):** Published on Mendeley Data under the Creative Commons Attribution 4.0 License (CC BY 4.0).  


## Acknowledgments
This work was originally inspired by the [GRBL project (GPLv3)](https://github.com/gnea/grbl). However, the code included here has been substantially rewritten and is distributed under the MIT License.  

