# Task 1: Understanding and implementing Verilog code on FM

#### How to run a profect in the FPGA board?
Follow the steps to run a project:

1. Navigate the specific folder

cd (project-folder)

2. Build the binaries

make build

3. Flash the code to external SRAM

sudo make flash

4. to clean

make clean




## Step 1: Understanding the verilog code
This is the link of the verilog code for the glowing of blue led in a RGB led present in the FPGA board. [top.v](https://github.com/Ahtesham18112011/VSDSquadron_FM/commit/c6511d8ea1d69d50770b938977da7150673a1d7a). 
<details>
  <summary>Module Analysis</summary>
  
  ![Alt text](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/a1070567667933317187255c10d645236658f859/Screenshot%20(87).png).
  
The first section of the verilog code says. 
  
1. **led_red,led_blue,led_green**  These are the output wires that controls the clors of RGB led which carries output of logic 1 or 0

2. **hw_clk**  It is a clock that provides clock signals to the module"s timing.

3. **testwire**  it is connected to bit 5 of the frequency counter

### Internal component Analysis
The module has three main internal components:-

1. **Internal Oscillator** It generates a internal clock signal

2. **Frequency counter** It has 28-bit register. Increments on every positive edge of int_osc. bit 5 is routed to the testwire 

3. **RGB led driver** It allows the frequency of red and green led the lowest and blue led the highest. it sets all the leds to the lowest.

   **Purpose**

   This verilog code for the FM allows it to glow a blue light in the RGB led in a controlled manner.

   **RGB LED driver functionality**

   The RGB LED driver manages the LED outputs

* Current controllled output with minimum current setting.
* Blue LED at maximum brightness.
* Red and green at minimum brightness.

  ## Step 2: Creating the PCF File
  This is the PCF file. [VSDSquadronFM.pcf](https://github.com/Ahtesham18112011/VSDSquadron_FM/blob/e42b59be2d586c9407dcfc91577753fcdb8994a9/VSDSquadronFM.pcf)
   

   


