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
  
  ***.lib*** stands for Liberty Timing File. 
The .lib file is an ASCII representation of the timing and power parameters associated with any cell in a particular semiconductor technology.It consists of timing and power parameters are obtained by simulating the cells under a variety of conditions (PVT) and the data is represented in the .lib format
The .lib file contains timing models and data to calculate
- I/O delay paths
- Timing check values
- Interconnect delays

We use an open source .lib file from Google Skywater sky130_fd_sc_hd__tt_025C_1v80.lib.


  ![Screenshot from 2022-08-07 22-43-29](https://user-images.githubusercontent.com/110079729/183976564-e517aff7-731b-4111-bbd1-0774d3fa6be6.png)
  
  
  ## 2.2 Hierarchical Synthesis and Flat Synthesis
  
  
  ### Hierarchical Synthesis
  
   When we perform hierarchical synthesis using the folloing commands,
```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr multiple_modules_synth.v
```

The verilog code for the same is:
![Screenshot from 2022-08-07 22-53-51](https://user-images.githubusercontent.com/110079729/183977897-7484ea6c-683e-4e2f-8946-1fd2603762eb.png)

The following fig shows the heirarchy of the multiple modules.

![Screenshot from 2022-08-07 23-00-44](https://user-images.githubusercontent.com/110079729/183978016-27b02b67-9d4f-41bc-8243-0bc9aca456d3.png)
  
  ### Flat Synthesis
  
  `flatten` is the command to flatten out the heirarchy and this is the resultant structure after removing heirarchy of the modules.

![image](https://user-images.githubusercontent.com/67214592/183256716-61d01cbf-a6b6-4214-bc4e-69067493ca5a.png)
  
  
  ## 2.3 Various Flip Flop coding Styles and Optimizations

In a combinational circuit, when the inputs are altered, the output changes after the circuit's propagation delay. If several pathways with differing propagation delays are used during data propagation, there may be a probability of an output glitch. Because more glitches arise when there are more combinational circuits in the design, the output becomes unstable. We are opting for flops to store the data from the communication lines in order to address this issue. When a flop is utilised, the output of the combinational circuit is stored in it and transmitted only at the positive or negative edge of the clock so that the subsequent combinational circuit receives a glitch-free input, stabilising the output. The set and reset control pins of a flop are used as initialization signals because without them, the following combinational circuit would get a trash value. Both synchronous and asynchronous control pins are possible.

### D Flip Flop with Asynchronous Reset
```
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

![Screenshot from 2022-08-08 07-42-11](https://user-images.githubusercontent.com/110079729/183980018-9f08cc28-1882-4bdd-ae5f-1f113e646978.png)

![Pic15](https://user-images.githubusercontent.com/110079729/183980334-476851f2-f745-42f6-beeb-c60d71509fce.png)


### D Flip Flop with Asynchronous Set
```
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

![Screenshot from 2022-08-08 07-44-37](https://user-images.githubusercontent.com/110079729/183980105-9b7318dc-18f4-4f59-b72f-d9b68621b318.png)

![Pic17](https://user-images.githubusercontent.com/110079729/183980385-7e981d40-9ae9-490e-a064-700884f3bf1b.png)


### D Flip Flop with Synchronous Reset
```
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
![Screenshot from 2022-08-08 07-50-30](https://user-images.githubusercontent.com/110079729/183980152-6e9a9840-9a12-434f-ab66-764fd4de1c55.png)

![Pic19](https://user-images.githubusercontent.com/110079729/183980419-ac43a41c-6383-4d49-963b-cf154410be02.png)

  
  ## 2.4 Interesting Optimizations
  
  The `verilog code` is given by the following and it is importnant to observe the inputs which are present inside the statement.  
	* In this hardware/standard cells is not necessary.

<pre><code>module mul2 (input [2:0] a, output [3:0] y);
   assign y = a * 2;
 endmodule</code></pre>
 
 **Synthesis schematic of the design.**   
 
 ![image](https://user-images.githubusercontent.com/67214592/183255244-f6f73d25-3aa4-4584-a5f0-2b03c301a188.png)   
 
 The `verilog code` is given by the following and it is importnant to observe the inputs which are present inside the statement.  

<pre><code>module mul2 (input [2:0] a, output [5:0] y);
   assign y = a * 9;
 endmodule</code></pre>
 
 **Synthesis schematic of the design.**
 
 ![image](https://user-images.githubusercontent.com/67214592/183255305-ad9db11d-064f-4337-a783-10b3a8719452.png)
  
  
# 3. Day-3- Combinational and Sequential Optimization

  ## 3.1 Introduction to Optimizations
  
  ### Combinational Logic Optimizations
Combinational logic optimizations helps in squeezing the logic to get the most optimized design. Also, optimized logic will be power and area efficient.
Techniques used for combinational logic optimization are 
 - Constant Propogation - direct optimization technique
 - Boolean Logic Optimization - KMap, Quinn Muclusky, etc

### Sequential Logic Optimizations
Sequential Logic Optimization techniques include
 - Basic - Sequential Constant Propogation
 - Advanced
    - State Optimization
    - Retiming
    - Sequential Logic Cloning (Floorplan aware synthesis)
  
  
  
  ## 3.2 Combinational Logic Optimizations
  
  The command used to perform logic optimization of combinational logic is `opt_clean -purge`. Here we consider the example opt_check.v,opt_check2.v, opt_check3.v, opt_check4.v and multiple_module_opt.v.

### Example: opt_check.v
```
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```
The commands to run optimizations are
```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check_synth.v
```
![Pic23](https://user-images.githubusercontent.com/110079729/183983409-67c0d4ed-8358-4ac9-ab99-7d739018eecd.png)



### Example: opt_check2.v
```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```
![Pic24](https://user-images.githubusercontent.com/110079729/183983454-cc6ed5d8-2c59-4116-84ea-b0430f67500c.png)


### Example: opt_check3.v
```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

![Pic25](https://user-images.githubusercontent.com/110079729/183983472-d52c2a17-73f5-4e62-9481-36c5724802cd.png)


### Example: opt_check4.v
```
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```
![Pic26](https://user-images.githubusercontent.com/110079729/183983504-46059805-ba1c-415f-b40b-cb1572c9a5b3.png)

  
  ## 3.3 Sequential Logic Optimizations
  
  ### Example-1
```
module dff_const1(input clk, input reset, output reg q);
	always @(posedge clk, posedge reset)
	begin
		if(reset)
			q <= 1'b0;
		else
			q <= 1'b1;
	end
endmodule
```
![Screenshot from 2022-08-09 23-04-03](https://user-images.githubusercontent.com/110079729/183984798-8307943b-2236-4ed6-8158-c6cc97f6b1f7.png)

![Screenshot from 2022-08-09 22-55-16](https://user-images.githubusercontent.com/110079729/183984927-a3c3444f-9daf-4af2-8402-e7ae87810391.png)


### Example-2
```
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end

endmodule
```
![Screenshot from 2022-08-09 22-56-50](https://user-images.githubusercontent.com/110079729/183985688-c77d3375-0aa6-4094-8cd9-23e1d7708c0e.png)

![Screenshot from 2022-08-09 23-12-52](https://user-images.githubusercontent.com/110079729/183985754-215cb0cf-6ced-4061-a469-f0f2840805d1.png)


### Example-3
```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```

![Screenshot from 2022-08-10 12-17-35](https://user-images.githubusercontent.com/110079729/183985917-6bc638c1-720f-4762-89dc-49276323c470.png)

![Screenshot from 2022-08-10 12-19-29](https://user-images.githubusercontent.com/110079729/183985980-b75c81ab-f8f6-4979-bbd1-92c2b18c24bf.png)

   
  
  ## 3.4 Sequential Optimization for unused outputs
  
  ### Example 
```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
Here the output of the module is just sensing the output q as count[0] Basically q toggles on every clock cycle. count[1] and count[2] have no functionality. So one d_ff and one inverter will realize the circuit.


  ![Screenshot from 2022-08-10 13-50-08](https://user-images.githubusercontent.com/110079729/183986335-680c879e-2468-4b34-bc7a-77da4fcd0650.png)

  

# 4. Day-4- GLS, Blocking vs Nonblocking and Synthesis-Simulation Mismatch

 
  ## 4.1 GLS Concepts And Flow
  
 GLS stands for gate level simulation. When we write the RTL code, we test it by giving it some stimulus through the testbench and check it for the desired specifications. Similarly, we run the netlist as the design under test (dut) with the same testbench. 
Gate level simulation is done to verify the logical correctness of the design after synthesis. Also, it ensures the timing of the design. 
 
  ![Pic38](https://user-images.githubusercontent.com/110079729/183990833-e2d590b2-51ef-4a50-8968-0de0d5a66655.png)

  ## 4.2 Synthesis Simulation Mismatch
  
  The possible reasons for Synthesis Simulation Mismatches are 
 - Missing Sensitivity List
 - Blocking and Nonblocking Assignments
 - Non-standard Verilog Coding Practices
  
  
  ## 4.3 Missing Sensitivity List
  
  The simulator works based on the activity, i.e., output will change only when input changes. Consider an example of a mux below.
```
module mux (input i0, input i1, input sel, output reg y);
	always@(sel)
		begin
			if(sel)
				y = i1;
			else
				y = i0;
		end
endmodule
```
Here, we did a major blunder. We have included only `sel` in the sensitivity list. Actually, we should have included `sel`, `i0` and `i1`. So, we get the following wrong waveform. 

![Pic39](https://user-images.githubusercontent.com/110079729/183992381-a6aa5396-3484-404d-9727-8055c26cc91a.png)


Alternatively, we can write this code, which works properly. 
```
module mux (input i0, input i1, input sel, output reg y);
	always@()
		begin
			if(sel)
				y = i1;
			else
				y = i0;
		end
endmodule
```

![Pic40](https://user-images.githubusercontent.com/110079729/183992456-41146b32-7433-4f5c-acb9-f1be3acb26a3.png)


  ## 4.4 Blocking and Nonblocking Statements in Verilog
  
  
  In a verilog code, blocking statements are in sequential order, where as non blocking statements are executed in concurrent fashion.

* Inside always block  
 `= Blocking`  
     * Executes the statement in the order it is written.  
     * So the first statement is evaluated before the second statement.  
  `<= Non-Blocking`  
     * Executes all RHS when always block is entered and assign to LHS.  
     * Parallel Evaluation.  
     
let us consider the follwoing `verilog code`.
```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule

```

In the above example the output d is evaluated first and x is evaluated next. Therefore the output d is evaluated based on the previous value of x. This creates a mismatch between Synthesis and Simulation output.

The following figure shows the `RTL simulation` output of above code. As it can be seen in the output waveform, the output y is evaluated based on the previous values of a and b.


![Screenshot from 2022-08-10 16-58-37](https://user-images.githubusercontent.com/110079729/184069274-6dbafba4-16e1-41c9-b8fd-b72d13d53790.png)




# 5. Day-5- If, case, for and for generate



  ## 5.1 If and Case Construct
  
  
  
  
  ## 5.2 Looping Constructs in Verilog



















