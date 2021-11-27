# MOS-VLSI-Circuit-Design
# ***_RTL-design-with-verilog-using-SKY130-Technology_***

***
<p align="center">
<img src="https://user-images.githubusercontent.com/54993262/119881189-a1ebc380-bf4a-11eb-9bdf-6cc93bbcf1bd.png" width="600" height="400">
</p>

***

# **_Table of Contents_**

* ## [Introduction to Verilog RTL design and Synthesis](https://github.com/Pramod-Krishna/RTL-design-with-verilog-using-SKY130-Technology#introduction-to-verilog-rtl-design-and-synthesis-1)

* ## [Timing libs, hierarchical vs flat synthesis and efficient flop coding styles](https://github.com/Pramod-Krishna/RTL-design-with-verilog-using-SKY130-Technology#timing-libs-hierarchical-vs-flat-synthesis-and-efficient-flop-coding-styles-1)

* ## [Combinational and sequential optmizations](https://github.com/Pramod-Krishna/RTL-design-with-verilog-using-SKY130-Technology#combinational-and-sequential-optmizations-1)

* ## [GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](https://github.com/Pramod-Krishna/RTL-design-with-verilog-using-SKY130-Technology#gls-blocking-vs-non-blocking-and-synthesis-simulation-mismatch-1)

* ## [Optimization in synthesis](https://github.com/Pramod-Krishna/RTL-design-with-verilog-using-SKY130-Technology#optimization-in-synthesis-1)
* ## [Acknowledgement](https://github.com/Pramod-Krishna/RTL-design-with-verilog-using-SKY130-Technology#acknowledgement)
* ## [Reference](https://github.com/Pramod-Krishna/RTL-design-with-verilog-using-SKY130-Technology#references)
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FPramod-Krishna%2FRTL-design-with-verilog-using-SKY130-Technology&count_bg=%233DC843&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=visitors&edge_flat=true)](https://hits.seeyoufarm.com)

***

# **_Introduction to Verilog RTL design and synthesis_**
Here **_iverilog_** is the simulator used. Simulator basically looks for changes in input, upon which output is evaluated. If there are no changes in the input, output won't be evaluated. **_iverilog_** takes **_design_** and **_testbench_** as inputs. **_Design_** is the actual verilog or set of verilog codes which has the intended funtionality to meet with the required specifications. **_Testbench_** is a setup that one uses to apply a set of stimuli to check the functional working of the design file. There are no primary inputs or outputs to testbench. 

These changes in input and corresponding output values are dumped in a special format file called value change dump(.vcd) file. The _waveforms_ can be viewed by using the **_gtkwave_** command to the _vcd file_.  

T

 

## _Implementing a Mux 2x1_


```
    iverilog good_mux.v tb_good_mux.v
    ./a.out
    gtkwave tb_good_mux.vcd
```
Verilog Code for Mux 2x1
![good_muxv](https://user-images.githubusercontent.com/54993262/120067755-03bb4300-c09b-11eb-9509-60ef05055bd5.JPG)
Test Bench for Mux 2x1
![tb_good_mux](https://user-images.githubusercontent.com/54993262/120067768-0fa70500-c09b-11eb-9472-50a334237857.JPG)
Output Waveforms
![gtk_good_mux](https://user-images.githubusercontent.com/54993262/120067779-1b92c700-c09b-11eb-81f8-c964f51609e8.JPG)


## _Sythesis using Yosys_
Yosys is a framework for Verilog RTL synthesis. It is a currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys can be invoked by simply typing yosys on the command prompt.
![yosys](https://user-images.githubusercontent.com/54993262/120067982-00748700-c09c-11eb-867c-1271acfc1af5.JPG)

```
      read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
      read_verilog good_mux.v
      synth -top good_mux
      abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
      show
```
The output can be seen below. The final sysnthesized netlist shows the _2:1 multiplexer_ RTL is translated to a gate level representation.
![good_mux_op_yosys](https://user-images.githubusercontent.com/54993262/120068133-c5bf1e80-c09c-11eb-8390-40ed07650a5c.JPG)

***

# _Timing libs, hierarchical vs flat synthesis and efficient flop coding styles_

To see what the .lib file contains:
``` 
      vim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```
This is the snapshot of the library.
![library](https://user-images.githubusercontent.com/54993262/120071322-60732980-c0ac-11eb-9e2f-a13b70f633f3.JPG)
The first line indicates the name of the library. **_tt_** represents the speed, the libraries can be slow, typical and fast. **__025C__** indicates the temperature and **__1v80__** the volatge.

We can also observe the area and power consumption of each cell in these libraries. 
Below are the three types of __2 input and gates__.
![and2_0](https://user-images.githubusercontent.com/54993262/120076152-c74f0d80-c0c1-11eb-90f2-efc5ef27f98e.JPG)
![and2_2](https://user-images.githubusercontent.com/54993262/120076154-c9b16780-c0c1-11eb-9e8d-576ad7a30a43.JPG)
![and2_4](https://user-images.githubusercontent.com/54993262/120076157-cc13c180-c0c1-11eb-9382-ad1cf6328864.JPG)

As seen, __and2_4__ has the least delay (fastest) but consumes more area, where as __and2_0__ has highest delay but takes less area.

*** 
## Hierarchical vs Flat Synthesis
This can be demonstrated by considering __multiple_modules.v__
![mult_mod](https://user-images.githubusercontent.com/54993262/120076437-26615200-c0c3-11eb-80ac-0f00c91193be.JPG)

Hierarchical sysnthesis is basically synthesizing using Yosys as shown previously. This type of sub_module level synthesis is known as Hierarchical Synthesis as the sub_modules are preserved in its hierarchy.
![mult_mod_op](https://user-images.githubusercontent.com/54993262/120076610-09794e80-c0c4-11eb-86a1-e74aa302da61.JPG)

```  
     write_verilog -noattr multiple_modules_hier.v
     !vim multiple_modules_hier.v
```
![mult_mod_hier](https://user-images.githubusercontent.com/54993262/120076955-a38dc680-c0c5-11eb-80f6-7fc831bbc987.JPG)
Here __OR__ gate is not implemented standardly as __NOR__ and inverter as stacking PMOS is bad. As PMOS has poor mobility, and to improve this we should design a wide cell. Thus it is implemented as __NAND__ gate with both inputs inverted. 

Inorder to obtain a gate based synthesized netlist file without preserving any of the sub_module hierarchy, we implement the Flat Synthesis technique using the flatten command. It can be written after right after the abc command.

```
     flatten
```
The output is seen as:
![m1](https://user-images.githubusercontent.com/54993262/120077296-6aeeec80-c0c7-11eb-840a-8d557124a0e3.JPG)

## Reasons for submodule level synthesis approach 
* When a design consists of multiple instances of the same module, we can use this and replicate the same for all the other instances of the same module and stitch it together to obtain the complete netlist file. This would save us some time and it is optimized.
* In case of massive designs, it can be split into submodules and independenlty synthezied. This is called divide and conquer approach.

***
## _Efficient Flip-flop coding styles and Optimizations_
Flipflops are memory elements. One important use is that they avoid glitch errors due to propagation delays between logic gates which cause instability in output. Let us look into D-flipflop implemented with async-reset, sync-reset and async set etc. Their verilog code can be viewed as follows:
![image](https://user-images.githubusercontent.com/54993262/120078360-cbccf380-c0cc-11eb-9eba-068f8c69586f.png)

***

# Acknowledgement
* 

***

# References
* 
