# FPGA board internship by Ahtesham Ahmed
### What is VSDSquadron FM (FPGA Mini) board?

The VSDSquadron FPGA(Field Programmable Gate Array) Mini (FM) is a compact and low-cost development board designed for FPGA prototyping and embedded system projects. This board provides a seamless hardware development experience with an integrated programmer, versatile GPIO access, and onboard memory, making it ideal for students, hobbyists, and developers exploring FPGA-based designs.[(source)](https://www.vlsisystemdesign.com/vsdsquadronfm/). 


#### How to run a project in the FPGA board?
Follow the steps to run a project:

**1.Navigate the specific folder**

```cd <project-folder>```

**2.Build the binaries**

```make build```

**3.Flash the code to external SRAM**

```sudo make flash```

4.**To clean**

```make clean```

# Task 1: Understanding the verilog code, creating the necessary PCF file and implementing in VSDSquadron FM

This task is divided into **four steps** or parts. 

**Step 1**: Understanding the verilog code 

**Step 2**: Creating the PCF File

**Step 3**: Integrating with the VSDSquadron FPGA Mini Board

**Step 4**: Final documentation

### Step 1: Understanding the verilog code
This is the link of the verilog code for the glowing of blue led in a RGB led present in the FPGA board. [top.v](https://github.com/Ahtesham18112011/VSDSquadron_FM/commit/c6511d8ea1d69d50770b938977da7150673a1d7a). 

<details>
  <summary>Analysis of the verilog code</summary>
  

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

  </details>


 

  ### Step 2: Creating the PCF File
  
  This is the PCF file. [VSDSquadronFM.pcf](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/e42b59be2d586c9407dcfc91577753fcdb8994a9/VSDSquadronFM.pcf). A PCF(Physical Constraint File) is a file which is used to instruct the FPGA to where it have to send the output, for example in this case of RGB LED the PCF file is used to instruct the FPGA to the RGB LED pins.

 <details>
  <summary>Analysis of the connection of the PCF file</summary> 

  


*  **set_io led_red 39**: This command helps the logical signal from FPGA to reach the pin number 39 which is one of the three input pins of thr RGB LED(which glows red led).

* **set_io led_blue 40**: This command helps the logical signal from FPGA to reach the pin number 40 which is one of the three input pins of thr RGB LED(which glows blue).

* **set_io led_green 41**: This command helps the logical signal from FPGA to reach the pin number 41 which is one of the three input pins of thr RGB LED(which glows green).

*  **set_io hw_clk 20** This command helps the logical signal from FPGA to reach the pin number 20.

*  **5 set_io testwire 17** This command helps the logical signal from FPGA to reach the pin number 17.

  <img src="https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/010ff4b0db3c8e0d270005114f78691f9bb029af/WhatsApp%20Image%202025-03-21%20at%202.38.37%20PM.jpeg" alt="Description" width="400"/>.

  </details>

  

### Step 3: Integrating with the VSDSquadron FPGA Mini Board

The [Datasheet](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/32ddb8c8ebc921e2051795b4388bbc49cba8ce46/VSDSquadronFMDatasheet.pdf) provide  details about the FPGA chip, SPI Flash Memory,USB to Serial converter etc. It also provides the steps to program the FPGA board, it explains all detail  about the FPGA board very clearly.

<details>
  <summary>Implementation in the FM</summary> 

According to the given [Datasheet](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/32ddb8c8ebc921e2051795b4388bbc49cba8ce46/VSDSquadronFMDatasheet.pdf). We need to do the following steps to implement the given verilog code in the FM:

1. Connect the board with the computer/laptop with a c type USB cable as described in the datasheet. Ensuring the FTDI connection. and type the command ```lsusb```. After typing this commmand you will see ”Future Technology Devices International” text in the terminal, it means the FPGA board is connected.

2. Make one more file which is called a Makefile.[Makefile](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/16f3657047eebb2d53e02e451deed799442105de/Makefile.txt).

3. Go to the software Ubuntu and in the terminal locate the file where you have made your PCF file,Verilog file and the Makefile. by pressing `cd <name of file>`

4. Ensure that there are no previous builds if there are then type `make clean`.

5. Then type `make build` to build the binaries.

6. Then type `sudo make flash` to program the FPGA. It will take some time.

7. When after this process you will see the blue LED glowing in the RGB LED.

  <img src="https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/main/WhatsApp%20Image%202025-03-18%20at%209.52.28%20PM.jpeg" alt="Description" width="500"/>

  </details>

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
* Difficulty in the software Ubuntu: You can download the Oracle Virtual box from its official website and follow the steps given in the datasheet to download the Ubuntu software.




  

  
  
   

   


