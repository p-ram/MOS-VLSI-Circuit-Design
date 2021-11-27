# MOS-VLSI-Circuit-Design WIP
# ***_6bit Left Shift using basic gates in Cadance Virtuoso_***

***
<p align="center">
<img src="https://github.com/p-ram/VLSI--6bit_Left_Shift/blob/main/ALU_block.jpg" width="600" height="400">
</p>

***

# **_Table of Contents_**

* ## [6bit_Shift Left](https://github.com/p-ram/VLSI--6bit_Left_Shift/tree/main/3row_6bit_LeftShift_Complete)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fp-ram%2FVLSI--6bit_Left_Shift&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

# _WIP_

```
     flatten
```
The output is seen as:
![m1](https://user-images.githubusercontent.com/54993262/120077296-6aeeec80-c0c7-11eb-840a-8d557124a0e3.JPG)

## Reasons for submodule level approach 
* When a design consists of multiple instances of the same module, we can use this and replicate the same for all the other instances of the same module and stitch it together to obtain the complete netlist file. This would save us some time and it is optimized.
* In case of massive designs, it can be split into submodules and independenlty synthezied. This is called divide and conquer approach.

***
## _Efficient Flip-flop coding styles and Optimizations_
Flipflops are memory elements. One important use is that they avoid glitch errors due to propagation delays between logic gates which cause instability in output. Let us look into D-flipflop implemented with async-reset, sync-reset and async set etc.
![image](https://github.com/p-ram/VLSI--6bit_Left_Shift/blob/main/ALU_block.jpg)

***

# Acknowledgement
* 

***

# References
* 
