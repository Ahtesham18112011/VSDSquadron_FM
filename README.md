# VSDSquadronFM FPGA board internship by Ahtesham Ahmed
### What is VSDSquadron FM (FPGA Mini) board?

The VSDSquadron FPGA(Field Programmable Gate Array) Mini (FM) is a compact and low-cost development board designed for FPGA prototyping and embedded system projects. This board provides a seamless hardware development experience with an integrated programmer, versatile GPIO access, and onboard memory, making it ideal for students, hobbyists, and developers exploring FPGA-based designs.[(source)](https://www.vlsisystemdesign.com/vsdsquadronfm/). 

 ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/ad31b442eb41918709c9383640ed51bc05f17ef6/VSDSquadronFM.png).
 
The VSDSquadron FM board - Features and specifications:

**FPGA:**
– Powered by the Lattice UltraPlus ICE40UP5K FPGA
– Offers 5.3K LUTs, 1Mb SPRAM, 120Kb DPRAM, and 8 multipliers for versatile design
capabilities

 **Connectivity:**
– Equipped with an FTDI FT232H USB to SPI device for seamless communication
– All FTDI pins are accessible through test points for easy debugging and customization

**General Purpose I/O (GPIO):**
– All 32 FPGA GPIOs brought out for easy prototyping and interfacing

 **Memory:**
– Integrated 4MB SPI flash for data storage and configuration

 **LED Indicators:**
– RGB LED included for status indication or user-defined functionality

 **Form Factor:**
– Compact design with all pins accessible, perfect for fast prototyping and embedded applications

> [!CAUTION]
> To avoid causing any damage or malfunctions, it is important to be mindful of the following pointswhen handling or operating the board: To prevent any damage, make sure to handle the board while taking electrostatic discharge(ESD) precautions. Power down the board by disconnecting the board from USB port.



#### How to run a project in the FPGA board?
Follow the steps to run a project:

**1.Navigate the specific folder**

       cd <project-folder>

**2.Build the binaries**

       make build

**3.Flash the code to external SRAM**

       sudo make flash

4.**To clean**

       make clean

       
### Dowmloading the required Open-source tools
Download the required tools for VSDSquadronFM. Project Icestorm, NextPNR and Yosys.
These Opensource tools can be downloaded by the following command:

#### Project icestorm

     git clone https://github.com/YosysHQ/icestorm.git icestorm
     cd icestorm
     make -j$(nproc)
     sudo make install

#### NextPNR

     git clone --recursive https://github.com/YosysHQ/nextpnr nextpnr
     cd nextpnr
     cmake -DARCH=ice40 -DCMAKE_INSTALL_PREFIX=/usr/local .
     make -j$(nproc)
     sudo make install

#### Yosys

     git clone https://github.com/YosysHQ/yosys.git yosys
     cd yosys
     make -j$(nproc)
     sudo make install

> [!NOTE]
> These are the commands for the language Linux.
   
# Tasks
****************************************************************************************************************************************************************
<details>
  <summary>Task 1: Implementing a verilog code to glow a blue LED in the RGB LED in the FM</summary>
  
### What this project do?

This project blinks a blue LED on the RGB LED present on the FPGA board.

### Step 1: Understanding the verilog code
This is the link of the verilog code for the glowing of blue led in a RGB led present in the FPGA board. [top.v(https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/0b4610393f38f0cda1ccef06b74bc228fa1ae707/Task%201/top.v). 

## Analysis of the verilog code
  

 ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a1070567667933317187255c10d645236658f859/Screenshot%20(87).png).
  
The first section of the verilog code says. 
  
1. **led_red,led_blue,led_green**  These are the output wires that controls the colors of RGB led which carries output of logic 1 or 0

2. **hw_clk**  It is a clock that provides clock signals to the module"s timing. It is the Hardware oscillator not the internal oscillator.

3. **testwire**  it is connected to bit 5 of the frequency counter as described below
   
    ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/8ad84dd438e48a361c21e7749db66f1531c2e4f1/Screenshot%20(89).png).
  

#### Internal component Analysis
The module has three main internal components:-

1. **Internal Oscillator(SB_HFOSC)** It generates a internal clock signal. Control Signals:
   
*    CLKHFPU = 1'b1 
*    CLKHFEN = 1'b1 
*    CLKHF (int_oscillator)

     ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/b79a55e797b72e8e7fe28e90f05d9f9165e3a30f/Screenshot%20(90).png).

3. **Frequency counter** It has 27-bit register because it is described as 'reg' in the verilog code, and reg means register. Increments on every positive edge of int_osc. bit 5 is routed to the testwire.

    ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/180c9374ec569df8b2e8ae465a5d46fe0d1766db/Screenshot%20(91).png).

5. **RGB led driver** It allows the frequency of red and green led the lowest and blue led the highest. it sets all the leds to the lowest.

     RGBLEDEN = 1'b1 : Enables LED operation
  
     RGB0PWM = 1'b0 : Red LED minimum brightness, as described in the verilog: 1'b0. In 1'b0 it is clearly seen that it is 1 bit binary zero value.
  
     RGB1PWM = 1'b0 : Green LED minimum brightness, as described in the given verilog: 1'b0. In 1'b0 it is clearly seen that it is 1 bit binary zero value.
  
     RGB2PWM = 1'b1 : Blue LED maximum brightness, as described in the given verilog: 1'b1. In 1'b1 it is clearly seen that it is a binary, unsigned, 1-bit wide integral value.

   It also allows the current to flow equally which is "0b000001" to RGB0(red), RGB1(green), RGB2(blue)

     ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/03bf86577080c878397fa207beafe230e47a3c23/Screenshot%20(95).png).


#### Purpose of the verilog code

This verilog code for the FM allows it to glow a blue light in the RGB led in a controlled manner.  It provides a stable internal clock source, It provides a complete solution for RGB LED control with built-in timing and test capabilities.

 #### RGB LED driver functionality

   The RGB LED driver manages the LED outputs

* Current controllled output with minimum current setting ("0b000001").
* Enables Blue LED at maximum brightness (1'b1).
* And Red and green at minimum brightness (1'b0).
* PWM (Pulse Width Modulation) control for each color.




 

  ### Step 2: Creating the PCF File
  
  This is the PCF file. [VSDSquadronFM.pcf](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/0b4610393f38f0cda1ccef06b74bc228fa1ae707/Task%201/VSDSquadronFM.pcf). A PCF(Physical Constraint File) is a file which is used to instruct the FPGA to where it have to send the output, for example in this case of RGB LED the PCF file is used to instruct the FPGA to the RGB LED pins.


  ## Analysis of the connection of the PCF file

  


* **set_io led_red 39**: This command helps the logical signal from FPGA to reach the pin number 39 which is one of the three input pins of thr RGB LED(which glows red led).

* **set_io led_blue 40**: This command helps the logical signal from FPGA to reach the pin number 40 which is one of the three input pins of thr RGB LED(which glows blue).

* **set_io led_green 41**: This command helps the logical signal from FPGA to reach the pin number 41 which is one of the three input pins of thr RGB LED(which glows green).

* **set_io hw_clk 20** This command helps the logical signal from FPGA to reach the pin number 20.

* **5 set_io testwire 17** This command helps the logical signal from FPGA to reach the pin number 17.

  <img src="https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/010ff4b0db3c8e0d270005114f78691f9bb029af/WhatsApp%20Image%202025-03-21%20at%202.38.37%20PM.jpeg" alt="Description" width="400"/>.

  

  

### Step 3: Integrating with the VSDSquadron FPGA Mini Board

The [Datasheet](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/32ddb8c8ebc921e2051795b4388bbc49cba8ce46/VSDSquadronFMDatasheet.pdf) provide  details about the FPGA chip, SPI Flash Memory,USB to Serial converter etc. It also provides the steps to program the FPGA board, it explains all detail  about the FPGA board very clearly.

> **Tip**

> Make sure you have downloaded the Ubuntu software. You can download it from Oracle Virtual box it does not take installing a new software.



## Implementation in the FM

According to the given [Datasheet](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/32ddb8c8ebc921e2051795b4388bbc49cba8ce46/VSDSquadronFMDatasheet.pdf). We need to do the following steps to implement the given verilog code in the FM:



1. Connect the board with the computer/laptop with a c type USB cable as described in the datasheet. Ensuring the FTDI connection. and type the command ```lsusb``` in the terminal of software Ubuntu. After typing this commmand you will see ”Future Technology Devices International” text in the terminal, it means the FPGA board is connected.

2. Make one more file which is called a Makefile.[Makefile](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/0b4610393f38f0cda1ccef06b74bc228fa1ae707/Task%201/Makefile.txt).

3. Go to the software Ubuntu and in the terminal locate the file where you have made your PCF file,Verilog file and the Makefile. by pressing `cd <name of file>`

4. Ensure that there are no previous builds if there are then type `make clean`.

5. Then type `make build` to build the binaries.

6. Then type `sudo make flash` to program the FPGA. It will take some time.

7. When after this process you will see the blue LED glowing in the RGB LED.



  <img src="https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/main/WhatsApp%20Image%202025-03-18%20at%209.52.28%20PM.jpeg" alt="Description" width="500"/>

  

### Step 4: Final documentation (Summary)
The given verilog code tells the three inputs of the RGB led with some internal and external devices like internal high-frequency oscillator and 28-bit frequency counter. The counter's bit 5 is routed to a testwire for monitoring. The RGB LED driver (SB_RGBA_DRV) provides current-controlled outputs with a fixed configuration: blue at maximum brightness, red and green at minimum.

**PCF file**
The [VSDSquadronFM.pcf](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/e42b59be2d586c9407dcfc91577753fcdb8994a9/VSDSquadronFM.pcf) is the file which contains the pin mapping of where the HDL code hhave to be gone. It is very important because it contains the details of where the code is to be gone. In the given PCF file codes of LED red,blue and green are connected to the pin 39,40 and 41 and the clock to pin 20 and lastly the code for testwire to the pin 17. 

**Implementing verilog code**
Follow the given [Datasheet](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/32ddb8c8ebc921e2051795b4388bbc49cba8ce46/VSDSquadronFMDatasheet.pdf). and connect the board to the computer and then go to the terminal and type `cd document name>` then `make build` and lastly `sudo make flash`. After the process you will see a blue light glowing on the RGB LED.

#### Final result

<img src="https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/main/WhatsApp%20Image%202025-03-18%20at%209.52.28%20PM.jpeg" alt="Description" width="500"/>.

## Challenges faced during the above process
* Face difficulty in connecting board: the USB-C cable was needed to connect. And connection between FTDI and the USB was also important,
* Difficulty in understanding verilog code: You can learn the language or search their meaning on google,firefox etc.

</details>

****************************************************************************************************************************************************************
 <details>
  <summary>Task2: Implementing a UART loopback mechanism on FM</summary>



    
## What is a UART?
UART, or Universal Asynchronous Receiver/Transmitter, is a hardware communication protocol that uses two wires (TX and RX) for transmitting and receiving serial data between devices, often used in embedded systems and microcontrollers. UART communication is asynchronous, meaning it doesn't rely on a shared clock signal between the sender and receiver. For UART to work, the Baud rate shoud be the same on both the transmitting anf receiving side

### Step 1: Studying the Existing code 
There are two verilog codes for this UART loopback mechanism.The first existing code for a uart_loopback mechanism can be found here [(top.v)](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/9617df7d78351e321941a7b556ba17ce3c103f22/uart-top.v). This is the second verilog code. [(uart_trx.v)](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/main/uart_trx.v)

## Analysis of the first veriog code
  
   ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/b2e72bae034c95a30bc69764fde0108752177795/Screenshot%20(94).png).
  
  The module of the verilog code explains four output and two input pins:
  
  1. **led_red led_blue led_green**: These are the three output wires that contriols the RGB LED.
  2. **uarttx**: This is the Transmission pin of the UART
  3. **uartx**: Thgis is the reciever pin of UART.

### Internal components analysis
**Internal Oscilliator** (SB_HFOSC)
It generates a internal clock signal. configuration:
*    CLKHFPU = 1'b1 
*    CLKHFEN = 1'b1 
*    CLKHF (int_oscillator)
  
**Frequency counter**
* It has 27-bit register because it is described as 'reg' in the verilog code, and reg means register. 
* Increments on every positive edge of int_osc.
* Bit 5 is routed to the testwire.

**UART**

In the Verilog code `assign uart_tx = uart_rx;`, the uart_tx signal is directly assigned the value of the uart_rx signal, effectively creating a loopback or echo where the transmitted data is immediately sent back to the receiver. 

 ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/1f5ff319e70d4d97d32e51df3e53ebec60939948/Screenshot%20(96).png).

 **RGB LED Driver**

It allows the frequency of red and green led the lowest and blue led the highest. it sets all the leds to the lowest.

* RGBLEDEN = 1'b1 : Enables LED operation
  
* RGB0PWM = 1'b0 : Red LED minimum brightness, as described in the verilog: 1'b0. In 1'b0 it is clearly seen that it is 1 bit binary zero value.
  
* RGB1PWM = 1'b0 : Green LED minimum brightness, as described in the given verilog: 1'b0. In 1'b0 it is clearly seen that it is 1 bit binary zero value.
  
* RGB2PWM = 1'b1 : Blue LED maximum brightness, as described in the given verilog: 1'b1. In 1'b1 it is clearly seen that it is a binary, unsigned, 1-bit wide integral value.

 
   
## Analysis of the second verilog code (uart.trx.v)  
 It is the verilog code for the **UART TX 8N1 Transmitter**.
 
#### Module  

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/b22fc42a132baec6250b7fad02d68d09ba566778/Screenshot%20(98).png).
 
 
The module explains 5 ports:
  
1. **clk**: input clock
    
2. **txbyte**: outgoing byte
    
3. **senddata**: trigger tx
    
4. **txdone**: outgoing byte sent
    
5. **tx**: tx wire


#### Input

The input explains three ports:

1.**clk**

2.**txbyte**

3.**senddata**

#### Output

The output explains two ports

1. **txdone**

2. **tx**

#### Parameters

**STATE_IDLE**: Waits for senddata.

**STATE_STARTTX**: Sends start bit (0).

**STATE_TXING**: Sends 8-bit data (LSB first).

**STATE_TXDONE**: Sends stop bit (1), marks completion.

### Step2: Creating a block diagram for UART loopback

### UART Loopback block diagram

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/0aa69637d1856f4aa88a26501098b5945f19bfcb/UART%20loopback.png).

### Detailed circut diagram of UART loopback

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/48651e2961e704b98d127f66c7c302d999cda0f4/Detailed%20circuit%20diagram%20UART%20loopback.png). 

### Step3: Implementation in the FM

> **Note**
> Create a  [Makefile](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/8e5519a421cbb128f586ade2d66ea6ae0c17c6d7/Makefile%20(UART%20loopback).txt) and paste it in the uart_loopback folder. Also ensure that the folder have the [PCF](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/b9d431c5828aba0c263ed9764659d42ec006338c/VSDSquadronFM%20(UART%20loopback).pcf) file and the two verilog codes.

Follow the steps to implement the verilog code on FM
1. Go to software  Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
2. Then navigate to the folder by typing `cd <folder name>`.
3. Then type `make build` to build the binaries.
4. Then type `sudo make flash` to program the board.
5. Now you have succesfully implemented the code in the FM.

### Step4: Testing and verification

We have implemented the necessary code and now we have to test that if it works or not. We will be using a serial terminal to test it. The serial terminal which we will be using is Docklight.
Follow the steps to test:

1. Go to Docklight and go to the project settings and set the Baud rate 9600.
2. Ensure the communication port in which the USB is connected to the FM and wright the COM number.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/396d554eb92322109637e356f7122ff34e5a6a6e/Testing1.png).    
3. Name the project name and wright the command which will be used in communication in sequence in the top left send sequences box.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/396d554eb92322109637e356f7122ff34e5a6a6e/Testing2.png).      
4. Then click on Apply.
5. Then click on the ---> sign at the send sequences box.
6. Then you will see the below results after the following results.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/396d554eb92322109637e356f7122ff34e5a6a6e/Testing3.png).

### Step5: Final Documentation

In UART (Universal Asynchronous Receiver/Transmitter) loopback, the transmitter's output is internally connected to the receiver's input, allowing a device to send data to itself for testing and troubleshooting. The TX (transmit) and RX (receive) lines are internally connected, so any data transmitted is also immediately received by the receiver within the same UART module. 

The given verilog cde basically explains the input and output pins of the module. The uarttx pin is connected to an output wire whereas the uartx pin is connected to an input pin it also explains the four parameters:
* STATE_IDLE: Waits for senddata.
* STATE_STARTTX: Sends start bit (0).
* STATE_TXING: Sends 8-bit data (LSB first).
* STATE_TXDONE: Sends stop bit (1), marks completion.

To understand the functioning of the uart loopback below are the block and circuit diagram of the uart loopback mechanism.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/0aa69637d1856f4aa88a26501098b5945f19bfcb/UART%20loopback.png).

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/48651e2961e704b98d127f66c7c302d999cda0f4/Detailed%20circuit%20diagram%20UART%20loopback.png). 

To implement the code on FM follow the following steps:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.

To test the results you can use any serial terminal but i am using Docklight.
1. Go to Docklight and go to the project settings and set the Baud rate 9600.
2. Ensure the communication port in which the USB is connected to the FM and wright the COM number.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/396d554eb92322109637e356f7122ff34e5a6a6e/Testing1.png).    
3. Name the project name and wright the command which will be used in communication in sequence in the top left send sequences box.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/396d554eb92322109637e356f7122ff34e5a6a6e/Testing2.png).      
4. Then click on Apply.
5. Then click on the ---> sign at the send sequences box.
6. Then you will see the below results after the following results.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/396d554eb92322109637e356f7122ff34e5a6a6e/Testing3.png).

## Challanges faced during the above process
* Difficulty in understanding verilog code: You can learn the language or search their meaning on google,firefox etc.
* Difficulty in identifying the communicacation port. follow the below steps.
1. Open Device Manager
2. Locate "Ports (COM & LPT)"
3. Identify and Note COM Ports

</details>

****************************************************************************************************************************************************************


 <details>
  <summary>Task3: Implementing a UART transmitter mechanism on FM</summary>

  
  ### What this project do?
  This project shows how to communicate with the PC and the FPGA it sends character D all time and if we press any key in our keyboard the D letter would not change because this is example is for transmitting the data to the PC not for receiving any data from the PC
  ### Step1: Studying the exsisting code
 These are the existing codes for the uart transmitter.[top.v](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/dc19cd95dd1d14183d73b8ce01c80c11a6c4d1c6/top%20(1).v) and [uart_trx.v](https://github.com/thesourcerer8/VSDSquadron_FM/blob/53840bb096ec59b11f26a0b5e362711b12540dbd/uart_tx/uart_trx.v). The uart_trx.v verilog code is same as the verilog code given in the task 2 therefore we will
 not be discussing it in this analysis. You can see the task 2 uart_trx.v analysis by going back.

 ### Analysis of the top.v code
 
 #### Module
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/c7f528012a595f554c5caf661d11a41862caef3e/Screenshot%20(107).png).

The module explains 5 ports 4 wires of output and a wire of input.
1. **led_red led_green led_blue**: These are the output wires that are connected to the RGB LED and controls the colors of the LED.
2. **uarttx**: This is the ouput wire which is connected to the output wire of the transmission pin.
3. **hw_clk**: This is the input wire of the mudule.  It is a clock that provides clock signals to the module"s timing. It is the Hardware oscillator not the internal oscillator.
   



* **uart_tx_8n1**: This is the name of the transmission pin.

* **DanUART**: This is the instance name of the uart_tx_8n1 transmission pin.
  
* **.clk (clk_9600)**: The clk input of the uart_tx_8n1 module is connected to the clk_9600 signal, which is a 9600 Hz clock generated within the top module.

* **.txbyte("D")**: The txbyte input of the uart_tx_8n1 module is connected to the character D. This is the data byte to be transmitted.

* **.senddata(frequency_counter_i[24])**: The senddata input of the uart_tx_8n1 module is connected to the 24th bit of the frequency_counter_i register. This signal likely triggers the sending of the txbyte.

* **.tx(uarttx)**: The tx output of the uart_tx_8n1 module is connected to the uarttx signal, which is the UART transmission pin.

**Overall, this module sets up a UART transmitter and controls RGB LEDs based on an internal oscillator and frequency counter.**

### Step2: Block and circuit diagram of the UART transmitter

**Block diagram:**

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/326bd802843f61d66d35b0c2c65d1783b01c2a8e/Screenshot%20(108).png).

**Circuit diagram**

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/ebce692adb70bfd8f7b661dbfd7408ced321bd84/Screenshot%20(109).png).


### Step3: Implementation on the board

> Make sure you have copied the following file:
> top.v,
> uart_trx.v,
> [Makefile](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/main/Makefile%20(UARTTX).txt) and 
> [PCF file](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/main/VSDSquadronFM%20(uarttx).pcf) and put all these in the folder that is created in the folder VSDSquadron_FM


To implement the code on FM follow the following steps:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.

### Step4: Testing and verification
To test, install PuTTY from its official webbsite it is a complete opensource software. Then after installing the software follow the below steps:-
1. Select the connection type as Serial, then you should check which COM port is working by taking a look in Device Manager.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(111).png)   
2. Click on "open".
3. Then you will see the folllowing 'D's after clicking:-

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(112).png)   

### Step5: Final documentation

The UART protocol is implemented im the module uart_trx.v file. It works in one direction only, ie. it sends data without having a provison to receive the data back from the receiver. For UART to work, the Baud rate shoud be the same on both the transmitting anf receiving side. Here the Baud rate is 9600 Hz.

The existing verilog code explains the fuctioning of the this project, the transmitting pin of the FPGA is named as uart_tx_8n1. It always sends the character 'D' all the time you can see above for more explaination.
We can understand the transmission of the FPGA by the following block and circuit diagram:
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/326bd802843f61d66d35b0c2c65d1783b01c2a8e/Screenshot%20(108).png).
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/ebce692adb70bfd8f7b661dbfd7408ced321bd84/Screenshot%20(109).png).

To implement the code on FM follow the following steps:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.

To test, install PuTTY from its official webbsite it is a complete opensource software. Then after installing the software follow the below steps:-
1. Select the connection type as Serial, then you should check which COM port is working by taking a look in Device Manager.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(111).png)   
2. Click on "open".
3. Then you will see the folllowing 'D's after clicking:-

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(112).png)   

As per the verilog code the FPGA is sending the character 'D' only.

## Challanges faced during the above process
* Difficulty in understanding verilog code: You can learn the language or search their meaning on google,firefox etc.
* Difficulty in PuTTY: If you are not able to to run the testing in PuTTY you can also run it in the Ubuntu software which use Linux language by the following steps:
Go to the terminal and type.

      sudo apt install picocom
      
this command will install the software called picocom
Then after installation type

       sudo make terminal
            
By this process you can also test it in the Ubuntu the outputs will be same only.

</details>

****************************************************************************************************************************************************************

 <details>
  <summary>Task4: Implementing a UART transmitter mechanism on FM which glows the LED  whenever a character is recieved from the PC</summary>

### What this project do?
It sends the 'D' characters repeatedly from the FPGA through USB to the computer, and lights up the LED whenever a character is received from the PC. 

### Step1: Understanding the existing verilog code

### Analysis of the top.v code
The existing verilog code for this project is [here](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/50504d14801f77a112a97e68f2fb0ed8d3ee39b0/top%20(sense).v)
  

 #### Module

The module has several ports:
 * **output wire led_red** declares an output port named led_red which is a wire.
 * **output wire led_blue** declares an output port named led_blue which is a wire.
 * **output wire led_green** declares an output port named led_green which is a wire.
 * **output wire uarttx** declares an output port named uarttx which is a wire. It is the transmission pin of the FPGA
 * **input wire uartrx** declares an input port named uartrx which is a wire. It is the receiver pin of the FPGA.
 * **input wire hw_clk** declares an input port named hw_clk which is a wire. It is the outer clock.

#### Internal Wires and Registers:
 * **nt_osc:** Wire for the internal oscillator.
 * **frequency_counter_i:** Register for counting the frequency.
 * **clk_9600 and cntr_9600:** Registers for generating a 9600 Hz clock from a 12 MHz clock.

#### UART Transmission pin

`uart_tx_8n1` is the transmissio pin of the FPGA, it sends the character 'D' continously.

#### Frequency Counter and Clock Generation:
* A counter increments on each positive edge of the internal oscillator (int_osc).
* A 9600 Hz clock is generated by counting up to period_9600.

### Step2: Block diagram and circuit for the verilog

**Block diagram**
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/326bd802843f61d66d35b0c2c65d1783b01c2a8e/Screenshot%20(108).png).

**Circuit diagram**

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/ebce692adb70bfd8f7b661dbfd7408ced321bd84/Screenshot%20(109).png).

### Step3: Implementation on the board

> Make sure you have copied the following file:
> top.v,
> [uart_trx.v](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/c3f163cc21c3779b0ebf307c8d70382f9013cbd1/uart_trx%20(3).v)
> [Makefile](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/c3f163cc21c3779b0ebf307c8d70382f9013cbd1/Makefile%20(3).txt) and 
> [PCF file](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/c3f163cc21c3779b0ebf307c8d70382f9013cbd1/VSDSquadronFM%20(3).pcf) and put all these in the folder that is created in the folder VSDSquadron_FM


To implement the code on FM follow the following steps:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.

After the programming, if you can see a red light on the RGB LED, you have successfully implemented the essential code for this project on the FM.

 ### Step4: Testing and verification
To test, install PuTTY from its official webbsite it is a complete opensource software. Then after installing the software follow the below steps:-
1. Select the connection type as Serial, then you should check which COM port is working by taking a look in Device Manager.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(111).png)   
2. Click on "open".
3. Then you will see the folllowing 'D's after clicking:-

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(112).png)   

### Step5; Final documentation

The top.v Verilog file defines a module named top which includes the following functionalities:

**Module Declaration**: The module has several output wires for LEDs (red, blue, green) and UART transmission (uarttx), and input wires for UART reception (uartrx) and hardware clock (hw_clk).

**Clock Generation**: A 9600 Hz clock is generated from a 12 MHz clock using a counter.

**UART Transmission**: The module uart_tx_8n1 is instantiated for UART transmission, configured to send the byte "D" when a specific condition is met.

**Internal Oscillator**: An internal high-frequency oscillator is instantiated using the SB_HFOSC primitive.

**Frequency Counter**: A counter increments on each positive edge of the internal oscillator to generate the 9600 Hz clock signal.

**RGB LED Driver**: The SB_RGBA_DRV primitive is used to control RGB LEDs, driven by the UART reception signal (uartrx).

In summary, the module handles clock generation, UART transmission, and RGB LED control.

These are the block and circuit diagram for this project to understand this clearly, you can observe that the block and circuit diagram for the case in task 2 is also as same as in the task 3 (this task). This is because this project is quite similar with the project in the task 2, the only difference is that This project lits up the LED in presence of the command from the keyboard and the previous one does not.

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/326bd802843f61d66d35b0c2c65d1783b01c2a8e/Screenshot%20(108).png).

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/ebce692adb70bfd8f7b661dbfd7408ced321bd84/Screenshot%20(109).png).

To implement the essential code follow the steps:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.

 ### Step4: Testing and verification
To test, install PuTTY from its official webbsite it is a complete opensource software. Then after installing the software follow the below steps:-
1. Select the connection type as Serial, then you should check which COM port is working by taking a look in Device Manager.
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(111).png)   
2. Click on "open".
3. Then you will see the folllowing 'D's after clicking:-

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a02b63cc8b04445e3aabc67e98a5ce367615749f/Screenshot%20(112).png)  

</details>


****************************************************************************************************************************************************************


 <details>
  <summary>Task5: Real-Time Sensor Data Acquisition and Transmission System</summary>


### What this project do?
This theme interfece with the sensors to receive the data sensed, and sending this data to the FPGA and then with the GPIO pins transmit the outpuit results to an external device (example buzzer). In this project we will make a tochless bell using the VSDSquadronFM board and with a sensor named HC-SR04 ultrasonic sensor.

### Requierd software and hardware components
**Hardware**
* VSDSquadronFM FPGA board
* HC-SR04 ultrasonic sensor
* A buzzer

**Software**
* Virtual Ubuntu software(for programming)
* Docklight

## Step1: Analysis of the existing verilog code 

The existing verilog code can be accessed [here](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/77683853b2e289a02d9d120a8bbb93b30b0d60f7/top%20(2).v). The top module integrates an ultrasonic sensor to measure distance, transmits the measured distance over UART, and controls RGB LEDs based on the distance.

### Module declaration

The mudule has several input and output ports:

**Outputs**
* `led_red, led_blue, led_green`: Control the RGB LEDs.
* `uarttx`: UART transmission pin.
* `trig`: Trigger output for the ultrasonic sensor.
* `buzzer`: Buzzer signal.

**Inputs**
* `uartx`: UART receiver pin.
* `hw_clk`: Hardware clock input.
* `echo`: Echo signal from the ultrasonic sensor.

### Internal Oscillator and counter 
Uses a high-frequency oscillator (SB_HFOSC) to generate a 12MHz clock signal (int_osc). And generates a 9600 Hz clock (clk_9600) using a counter (cntr_9600) to divide the 12MHz clock.

### Ultasonic senser signals
Declares signals for distance measurement and sensor control:

* `distanceRAW, distance_cm`: Raw and processed distance values.
* `sensor_ready`: Indicates if the sensor is ready.
* `measure`: Control signal for measurement.
* `buzzer_signal`: Signal to control the buzzer.

### Finite state machine to print distance_cn as ASCII

Implements an FSM to convert the distance measurement (distance_cm) into ASCII characters and transmit via UART:
States:
* `IDLE`: Waits for sensor readiness.
* `DIGIT_4 to DIGIT_0`: Converts each digit of the distance value to ASCII.
* `END_CR`: Sends carriage return (CR).
* `SEND_LF`: Sends line feed (LF).
* `DONE`: Completes the transmission.
  
**The top module integrates an ultrasonic sensor to measure distance, transmits the measured distance over UART, and controls RGB LEDs based on the distance. The FSM ensures that the distance is converted to ASCII and sent over UART, and the LEDs provide a visual indication of proximity.**

### Analysis of the Ultra sonic sensor module (ultra_sonic_sensor.v)
The verilog code for the HC-SR04 module can be accessed [here](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/37f3bca7f6c761e938fda76e942c5f861b84f79e/ulta_sonic_sensor.v).

### Module declaration
The module explains some input and output ports:
* `clk`: System clock input.
* `measure`: Signal to start the measurement.
* `state`: Current state of the finite state machine (FSM).
* `ready`: Indicates if the module is ready for a new measurement.
* `echo`: Echo signal from the ultrasonic sensor.
* `trig`: Trigger signal to the ultrasonic sensor.
* `distanceRAW`: Raw distance measurement in clock cycles.
* `distance_cm`: Converted distance in centimeters.
* `buzzer_signal`: Signal to activate a buzzer if the distance is less than or equal to 5 cm.

### State definitions
* `IDLE`: Waiting for a measurement pulse.
* `TRIGGER`: Sending the trigger pulse.
* `WAIT`: Waiting for the echo signal.
* `COUNTECHO`: Counting the duration of the echo signal.

### Finite State Machine (FSM)
* The FSM transitions between states based on the `measure` signal and the `echo` signal.
* In the `TRIGGER` state, it generates a trigger pulse.
* In the `WAIT` state, it waits for the echo signal to go high.
* In the `COUNTECHO` state, it counts the duration of the echo signal.

### Distance Measurements
* The `distanceRAW` counter increments while the echo signal is high.
* The raw distance is then converted to centimeters using the formula: `distance_cm = (distanceRAW * 34300) / (2 * 12000000)`.
* If the distance is less than or equal to 5 cm, the `buzzer_signal` is activated.

### Refresher module
The `refresher50ms` module generates a measurement pulse every 50 milliseconds.

### Step2:  Block diagram of the pin connection of the project and the processing flow

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/f3bd45dedf4555f63e673ebcabb1230cd9df8dea/Screenshot%20(113).png)

**Internal processing flow**

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/226e757b519b9e607027ee589a2230a9f2b51540/Screenshot%20(114).png)

### Step3: Implementation in the FM

> Make sure you have copied the following file:
> top.v,
> ultra_sonic_sensor.v
> [uart_trx.v](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/c3f163cc21c3779b0ebf307c8d70382f9013cbd1/uart_trx%20(3).v)
> [Makefile](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/c3f163cc21c3779b0ebf307c8d70382f9013cbd1/Makefile%20(3).txt) and 
> [PCF file](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/829aacc385a798ed2cccd673f3802d2e45ffff28/VSDSquadronFM%20(4).pcf) and put all these in the folder that is created in the folder VSDSquadron_FM named touchless_bell.


To implement the code on FM follow the following steps:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.

### Step4: Testing and verification

Follow the steps to test it in the software Docklight:
1. Set the baud rate to 9600. And 1 stop bit
2. Ensure the port through which the FM is connected in the device manager
3. Then test with objects. Put the object or anything above the senser, the buzzer will beep when the object is kept at a feild of 5cm

### Step5: Final documentation

The project has 3 verilog codes including the top.v.This module integrates sensor data acquisition, signal processing, and communication to provide a comprehensive functionality for an FPGA-based system. And also a verilog named ultra_sound_sensor.v. This code sets up the HC-SR04 sensor to measure distance and trigger a buzzer if the measured distance is below a specific given vvoltage.

The pins of the ultrasonic sensor can be connected like the following way:

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/f3bd45dedf4555f63e673ebcabb1230cd9df8dea/Screenshot%20(113).png)

The internal processing diagram can be made by this way

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/226e757b519b9e607027ee589a2230a9f2b51540/Screenshot%20(114).png)

Run the following steps to implement the codes:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.

Test it by by using a serial terminal likr Dochlight by the steps:
1. Set the baud rate to 9600. And 1 stop bit
2. Ensure the port through which the FM is connected in the device manager
3. Then test with objects. Put the object or anything above the senser, the buzzer will beep when the object is kept at a feild of 5cm

</details>

****************************************************************************************************************************************************************

<details>
  <summary>Task6: BCD to 7-segment display converter</summary>

### What this project do?
A BCD to 7-segment converter, also known as a BCD to 7-segment decoder, is a circuit that converts a binary-coded decimal (BCD) input into a signal that can drive a 7-segment display. The 7-segment display is a common electronic display device used to show decimal digits. This project is used to implement verilog code in the VSDSquadronFM to display the decimal digits in the 7-segment-display.

### Requierd software and hardware components
**Hardware**
* VSDSquadronFM
* a common cathode 7-segment-display
* breadboard
* male to male and male to female jumper wires
* 3V battery

**Software**
* Only the virtual Ubuntu software

## Step1: Analysis of the existing verilog code 
This is the verilog code for a BCD (Binary Coded Decimal) to 7 segment display converter. [top.v](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/97211f8b7b507d75259ca82fd385ca6edf626eda/7segment.png)

**Analysis of the verilog code**

       //This is the verilog module for a BCD to seven segment dispalay decoder.
       //This module takes a 4 bit BCD input and converts it to a 7 segment display output.
       module top(
       bcd,
       seg
       );
     
       //Declare inputs,outputs and internal variables.
       input [3:0] bcd;
       output [6:0] seg;
       reg [6:0] seg;

       //always block for converting bcd digit into 7 segment format
       always @(bcd)
        begin
          case (bcd) //case statement
              0 : seg = 7'b0000001;
              1 : seg = 7'b1001111;
              2 : seg = 7'b0010010;
              3 : seg = 7'b0000110;
              4 : seg = 7'b1001100;
              5 : seg = 7'b0100100;
              6 : seg = 7'b0100000;
              7 : seg = 7'b0001111;
              8 : seg = 7'b0000000;
              9 : seg = 7'b0000100;
               //switch off 7 segment character when the bcd digit is not a decimal number.
               default : seg = 7'b1111111; 
           endcase
      end
    
      endmodule

**Module**

                 module top(
                 bcd,
                 seg
                 );
      
**module**: A keyword in Verilog used to define a module (basic building block in Verilog).

**top**: The name of the module.

**(bcd, seg)**: The input and output ports of the module. Here, bcd is the input, and seg is the output.

**Inputs and outputs**

                input [3:0] bcd;
                output [6:0] seg;
                reg [6:0] seg;

**input [3:0] bcd**: Declares bcd as a 4-bit input. It represents a Binary Coded Decimal (BCD) digit (values 0–9).`[3:0]`: Specifies that bcd is 4 bits wide, with indices ranging from bit 3 to bit 0.

**output [6:0] seg**: Declares seg as a 7-bit output. It represents the 7 segments (a, b, c, d, e, f, g) of a 7-segment display.

**reg [6:0] seg**: Declares seg as a 7-bit register type. Since seg is assigned inside an always block, it must be declared as a reg.

**Always block**

                 always @(bcd)

**always**: A keyword in Verilog used to define a procedural block that executes whenever a specified event occurs.

**@(bcd)**: Specifies that the always block will execute whenever there is any change in the bcd input.


**Case statement**

                 case (bcd) //case statement
            
**case (bcd)**: A case construct is used to define multiple conditions. Here, the value of bcd determines which block of code will execute.

**case conditions**

                 0 : seg = 7'b0000001;
                 1 : seg = 7'b1001111;
                 2 : seg = 7'b0010010;
                 3 : seg = 7'b0000110;
                 4 : seg = 7'b1001100;
                 5 : seg = 7'b0100100;
                 6 : seg = 7'b0100000;
                 7 : seg = 7'b0001111;
                 8 : seg = 7'b0000000;
                 9 : seg = 7'b0000100;

Each line assigns a 7-bit binary value to seg for a specific bcd value. These binary values control which of the 7 segments (a, b, c, d, e, f, g) of a 7-segment display are turned ON (0) or OFF (1).
Example: 0 : seg = 7'b0000001;

**0**: Condition when bcd is 0.

**seg** = 7'b0000001: Assigns the binary value 0000001 to seg.

**7'b**: Indicates a 7-bit binary value.

**0000001**: Turns ON segments a, b, c, d, e, f (to display "0") and turns OFF segment g.

**Default case**

                  default : seg = 7'b1111111; 
                  
`default`: Specifies the action to take when bcd does not match any of the defined conditions (i.e., values other than 0–9).

`seg = 7'b1111111`: Turns OFF all segments of the 7-segment display.


**This code is typically used in digital circuits where numeric data needs to be displayed on a 7-segment display, such as digital clocks, counters, and calculators.**

## Step3: Analysis of the PCF file


    set_io bcd[0] 42
    set_io bcd[1] 43
    set_io bcd[2] 44
    set_io bcd[3] 45
    set_io seg[0] 46
    set_io seg[1] 13
    set_io seg[2] 48
    set_io seg[3] 3
    set_io seg[4] 4
    set_io seg[5] 12
    set_io seg[6] 26
    
This is the PCF file for the project. There are 4 input pins (bcd[0],bcd[1],bcd[2],bcd[3]) and seven output pins (seg[0],seg[1],seg[2],seg[3],seg[4],seg[5],seg[6]). You can set the GPIO pins by yourself.

## Step3: Block and circuit diagrams.

**Block diagram**
  
![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/09084ffe8efd977a44b4cb9bcb718faea4c986ce/Screenshot%20(118).png)

**Circuit diagram**

![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/09084ffe8efd977a44b4cb9bcb718faea4c986ce/Screenshot%20(119).png)

## Step4: Implementing in the VSDSquadronFM


> Make sure you have copied the following file:
> top.v,
> [Makefile](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/c3f163cc21c3779b0ebf307c8d70382f9013cbd1/Makefile%20(3).txt) and 
> PCF file and put all these in the folder that is created in the folder VSDSquadron_FM named 7_segement.



To implement the code on FM follow the following steps:
* Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing `lsusb`.
* Then navigate to the folder by typing `cd <folder name>`.
* Then type `make build` to build the binaries.
* Then type `sudo make flash` to program the board.
* Now you have succesfully implemented the code in the FM.
  
## Step5: Testing and verification

Connect the GPIO pins to the input pins of the 7 segment display as follows:

|Pins of the 7 segment display| GPIO pins| 
|---|-------------|
| a | 46          |
| b | 13            | 
| c | 48 | 
| d | 3 |   
| e | 4 | 
| f | 12 | 
| g | 26 |  

You can place four DIP switches or four pushbuttons in the input side to control the decimal digits presented on the 7-segment-display. If you are getting the  decimal numbers , you have successfully completed this project.

<img src="https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/3d5c31db2bd6333e3cde69f14cbbb12f9e636fba/ni7.png" alt="Description" width="500"/>.
