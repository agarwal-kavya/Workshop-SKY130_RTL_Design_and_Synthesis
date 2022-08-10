# Workshop-SKY130_RTL_Design_and_Synthesis

A report on 5 day workshop on RTL design and synthesis using opensource tools - iverilog, gtkwave, yosys and sky130-130nm technology PDK and standard cell library.

# Table of contents

- [Day-1- Introduction to Verilog RTL Design and Synthesis](#1-Introduction-to-Verilog-RTL-Design-and-Synthesis)
    - [1.1 Introduction to iverilog and gtkwave based simulation](#12-Introduction-to-iveriilog-and-gtkwave-based-simulation)
    - [1.2 Introduction to Yosys and Logic Synthesizer](#13-Introduction-to-Yosys-and-Logic-Synthesizer)
 - [Day-2- Timing Libraries, Hierarchical and Flat Synthesis](#2-Timing-Libraries-Hierarchical-and-Flat-Synthesis)
    - [2.1 Introduction to Timing Libraries](#21-Introduction-to-Timing-Libraries)
    - [2.2 Hierarchical Synthesis and Flat Synthesis](#22-Hierarchical-Synthesis-and-Flat-Synthesis)
    - [2.3 Various Flip Flop coding Styles and Optimizations](#23-Various-Flip-Flop-coding-Styles-and-Optimizations)
    - [2.4 Interesting Optimizations](#24-Interesting-Optimizations)
 - [Day-3- Combinational and Sequential Optimization](#3-Combinational-and-Sequential-Optimization)
    - [3.1 Introduction to Optimizations](#31-Introduction-to-Optimizations)
    - [3.2 Combinational Logic Optimizations](#32-Combinational-Logic-Optimizations)
    - [3.3 Sequential Logic Optimizations](#33-Sequential-Logic-Optimizations)
    - [3.4 Sequential Optimization for unused outputs](#34-Sequential-Optimization-for-unused-outputs)
 - [Day-4- GLS, Blocking vs Nonblocking and Synthesis-Simulation Mismatch](#4-GLS-Blocking-vs-Nonblocking-and-Synthesis-Simulation-Mismatch)
    - [4.1 GLS Concepts And Flow](#41-GLS-Concepts-And-Flow)
    - [4.2 Synthesis Simulation Mismatch](#42-Synthesis-Simulation-Mismatch)
    - [4.3 Missing Sensitivity List](#43-Missing-Sensitivity-List)
    - [4.4 Blocking and Nonblocking Statements in Verilog](#44-Blocking-and-Nonblocking-Statements-in-Verilog)
 - [Day-5- If, case, for and for generate](#5-If-case-for-and-for-generate)
    - [5.1 If and Case Construct](#51-If-and-Case-Construct)
    - [5.2 Looping Constructs in Verilog](#52-Looping-Constructs-in-Verilog)

 - [Acknowledgement](#6-Acknowledgment)


# 1. Day-1- Introduction to Verilog RTL Design and Synthesis

Register Transfer Logic (RTL) is used to capture logic in design phase of the integrated circuit design cycle. A logic synthesis tool converts an RTL description written in Hardware Description Language (Verilog/VHDL) to a gate-level description of the circuit. Placement and routing tools use the synthesis outputs to generate a physical layout.

### Introduction to Simulator

Icarus Verilog(iverilog) is a Verilog simulator used to verify functional description of a design. It functions as a compiler, converting Verilog source code into a target format. VCD (Value Change Dump) is a standard dump format for Verilog that dumps the status of the design as it simulates. The iverilog simulator takes a verilog design file and its test bench as inputs.

Simulator is a tool used for checking a verilog design. RTL design is basically the implementation of the design specifications. RTL design is checked for adherence to the specifications by simulating the design. Simulator looks for changes on the input signals. Upon any chaange to the input, output is evaluated. If there are no changes in the input, simulator will not evaluate the output.

![Pic1](https://user-images.githubusercontent.com/110079729/183963251-92f12291-8895-464b-8587-34a9bf2dbcee.png)

Here, design will be instantiated in the testbench so that there is a mechanism to apply the stimulus. Design may have one or more primary inputs and one or more primary outputs. Whereas the testbench has no primary inputs and primary outputs.


  ## 1.1 Introduction to iverilog and gtkwave based simulation
  
  Create a folder VLSI workshop. Move into this folder and clone the git repository https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git.
```
cd VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop
```

Here, a folder called `my_lib` is created, which contains subfolder `verilog_model` and a folder called `lib` is created. `lib` contains the sky130 stdcell library which is used for synthesis. `verilog_model` contains standard cell verilog models on the cells present in the `lib` file. `verilog_files` contains all the lab experiments needed for the workshop.

Iverilog takes the design and testbench as the input and generates a `vcd` file as the output. This `vcd` file stands for value change dump which can be opened with `gtkwave`.
  
  ![183249132-c009f5ee-4e72-48af-b6c3-8b6ec0cfcbec](https://user-images.githubusercontent.com/110079729/183962812-e0f0551a-eeef-4666-b723-71575026061f.png)
  
 
  ## 1.2 Introduction to Yosys and Logic Synthesizer
  
  Yosys is an open source RTL synthesis tool which takes verilog file of a design and technology library file(.lib) as inputs and generates gate level netlist corresponding the functional behaviour of the design.  
* Yosys Synthesizer tool used for converting the RTL to Netlist.
* Netlist is the representation of the design in form of the standard cell in `.lib`.

![image](https://user-images.githubusercontent.com/67214592/183249785-18fb67ab-f770-4390-88ea-a0cd0de73ad9.png)

To Verify the Synthesis:  
  *The testbench used in synthesis should be same as the RTL testbech. The output of the gtkwave should be same as output observed during RTL simulation.*

![image](https://user-images.githubusercontent.com/67214592/183249738-8b4011cc-cb80-4751-ac1c-203e06a18a82.png)

#### **Logic Synthesis**

![image](https://user-images.githubusercontent.com/67214592/183250146-702c3093-add2-48ca-87b4-65dd08282aa7.png)

1. RTL Design  
* It is the behavioral representation of the required specification.  
2. .lib  
* It has collection of logical modules.  
* It also includes basic logic gtes like AND, OR, NOT etc.,  
* Contains information about standard cell such area, power and timing, characterised for different PVT conditions.
* Guide the synthesizer to select the flavoured of gate cells like `slow cell`, `medium cell`, `fast cell`, for optimum implementation of logic circuits.  
3. Synthesis
* Synthesis is the RTL to gate level translation.  
* The design is converted into gates and the connections are made between the gates.  
* This is given out as a file called Netlist.  

### Table 1.1 : List of commands used in RTL Simultion  
|**Commands**|**Description**|
| :-: | :-: |
|  `iverilog verilogfilename.v tb_verilogfilename.v` |<p>To compile verilog design file and testbench file .</p><p>Ex: iverilog and21.v tb\_and21.v</p>|
|  `./a.out`|To generate VCD file|
|  `gtkwave tb_verilogfilename.vcd` |To invoke the waveform|

### Table 1.2 : List of commands used in Synthesis  
|**Commands**|**Description**|
| :-: | :-: |
|`Yosys`|To invoke open source logic synthesizer|
|`read_liberty -lib ../my_lib/lib/sy130_fd_sc_hd__tt_025C_1V80.lib`|To read .lib file |
|`read_verilog verilogfilename.v`|To read verilog design file|
|`synth -top verilogfilename`|To synthesize by setting a design as a top modules, if there are one or more submodules.|
|`abc -liberty ../my_lib/lib/sy130_fd_sc_hd__tt_025C_1V80.lib`|To generate gate level netlist|
|`write_verilog -noattr  verilogfilename_netlist.v`|To write netlist into a verilog file.|
|`show`|To display gate level schematic of the design|
  
### Lab Example using iverilog, gtkwave and yosys

**iverilog - good_mux.v**

![good_mux_v](https://user-images.githubusercontent.com/67214592/183285672-b0172a43-cbbb-476f-93cf-1dcf8461f4d1.PNG)

**RTL Simulation**

![Screenshot from 2022-08-07 17-31-27](https://user-images.githubusercontent.com/110079729/183973538-4ee038ce-b891-482d-a727-98513dc93878.png)

**Synthesis**

![image](https://user-images.githubusercontent.com/67214592/183291812-b95d850c-3194-4b58-934e-d90a394b836b.png)

**Netlist**

![Screenshot from 2022-08-07 18-27-58](https://user-images.githubusercontent.com/110079729/183974068-35461ad1-374c-4091-bca0-a5407d5e4055.png)


# 2. Day-2- Timing Libraries, Hierarchical and Flat Synthesis



  ## 2.1 Introduction to Timing Libraries
  
  
  ## 2.2 Hierarchical Synthesis and Flat Synthesis
  
  
  ## 2.3 Various Flip Flop coding Styles and Optimizations
  
  
  
  ## 2.4 Interesting Optimizations
  

# 3. Day-3- Combinational and Sequential Optimization

  ## 3.1 Introduction to Optimizations
  
  
  ## 3.2 Combinational Logic Optimizations
  
  
  ## 3.3 Sequential Logic Optimizations
  
  
  
  ## 3.4 Sequential Optimization for unused outputs
  
  

# 4. Day-4- GLS, Blocking vs Nonblocking and Synthesis-Simulation Mismatch
  
  ## 4.1 GLS Concepts And Flow
  
  
  
  ## 4.2 Synthesis Simulation Mismatch
  
  
  
  ## 4.3 Missing Sensitivity List
  
  
  
  ## 4.4 Blocking and Nonblocking Statements in Verilog
  
  
  


# 5. Day-5- If, case, for and for generate



  ## 5.1 If and Case Construct
  
  
  
  
  ## 5.2 Looping Constructs in Verilog



















