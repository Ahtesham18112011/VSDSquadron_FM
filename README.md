# Task 1: Understanding and implementing Verilog code on FM

#### How to program the FPGA board?
## Understanding the verilog code
This is the link of the verilog code for the glowing of blue led in a RGB led present in the FPGA board. [top.v](https://github.com/Ahtesham18112011/VSDSquadron_FM/commit/c6511d8ea1d69d50770b938977da7150673a1d7a). 
<details>
  <summary>Module Analysis</summary>
The first section of the verilog code says. 
  
1. **led_red,led_blue,led_green**  These are the output wires that controls the clors of RGB led which carries output of logic 1 or 0

2. **hw_clk**  It is a clock that provides clock signals to the module"s timing.

3. **testwire**  it is connected to bit 5 of the frequency counter

### Internal component Analysis
The module has three main internal components:-

1. **Internal Oscillator** It generates a internal clock signal

2. **Frequency counter** It has 28-bit register. Increments on every positive edge of int_osc. bit 5 is routed to the testwire 

3. **RGB led driver** It allows the frequency of red and green led the lowest and blue led the highest. it sets all the leds to the lowest.


