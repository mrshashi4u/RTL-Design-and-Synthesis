# Table of contents
1. [Introduction to RTL design and Synthesis](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/README.md#1-introduction-to-rtl-design-and-synthesis)
2. [Open Source tools for RTL Design and Synthesis](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#2-open-source-tools-for-rtl-design-and-synthesis)
    1. [iverilog](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#i--iverilog)
    2. [GTKWAVE](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#ii-gtkwave)
    3. [YOSYS](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#iii--yosys)
3. [Flop coding style and optimization techniques](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#3-flop-coding-style-and-optimization-techniques)
    1. [Flop with asynchronous set/reset](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#flop-with-asynchronous-setreset)
    2. [Flop with synchronous set/reset](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#flop-with-synchronous-setreset)
    3. [Flop with asynchronous and synchronous set/reset](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#flop-with-synchronous-and-asynchronous-setreset)


# RTL-Design-and-Synthesis using opensource Skywater130 PDK

## **1. Introduction to RTL design and Synthesis**
Register Transfer Logic (RTL) is used to capture logic in design phase of the integrated circuit design cycle. A logic synthesis tool converts an RTL description written in Hardware Description Language (Verilog/VHDL) to a gate-level description of the circuit. Placement and routing tools use the synthesis outputs to generate a physical layout. The following figure shows the flow of RTL synthesis. 
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/My%20drawings/RTL%20design%20flow.PNG)

## **2. Open Source tools for RTL Design and Synthesis**

### **I.  iverilog:** 
Icarus Verilog(iverilog)* is a Verilog simulator used to verify functional description of a design. It functions as a compiler, converting Verilog source code into a target format. VCD (Value Change Dump) is a standard dump format for Verilog that dumps the status of the design as it simulates. The iverilog simulator takes a verilog design file and its test bench as inputs. The following figure illustrates the inputs and outputs of iverilog simulator.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/My%20drawings/iverilog.PNG)

Table 2.1 Input files description of iverilog simulator

**Input file/files**|**Description**|
| :-: | :- |
|<p>**Verilog file**</p><p></p>|<p>Contains functional description of a design in HDL. **Ex: 2:1 Mux verilog file.<pre><code>**</p></p>*module m21(i0, i1,sel, y);*</p>*output y;*</p>*input i0, i1, sel;*</p><p>*assign y=(sel)?i1:i0;*</p><p>*endmodule*</p><p><code><pre>|
|<p>**Verilog Test Bench**</p><p></p>|<p>Contains stimulus information for the design **Ex: 2:1  Mux test bench**</p><p>‌ <pre><code>//timescale 1ns / 1ps</p><p>*module tb\_good\_mux;*</p><p>*// Inputs*</p><p>*reg i0,i1,sel;*</p><p>*// Outputs*</p><p>*wire y;*</p><p></p><p> *// Instantiate the Unit Under Test (UUT)*</p><p>*good\_muxuut(.sel(sel),.i0(i0),.i1(i1),.y(y));*</p><p></p><p>*initial begin*</p><p>*$dumpfile("tb\_good\_mux.vcd");*</p><p>*$dumpvars(0,tb\_good\_mux);*</p><p>*// Initialize Inputs*</p><p>*sel = 0;*</p><p>*i0 = 0;*</p><p>*i1 = 0;*</p><p>*#300 $finish;*</p><p>*end*</p><p></p><p>*always #75 sel = ~sel;*</p><p>*always #10 i0 = ~i0;*</p><p>*always #55 i1 = ~i1;*</p><p>*endmodule*</p><code><pre>|

Table 2.2 : List of commands used in iverilog
|**Commands**|**Description**|
| :-: | :-: |
|  `iverilog` |<p>To compile verilog design file and testbench file .</p><p>Ex: iverilog and21.v tb\_and21.v</p>|
|  `./a.out`|To generate VCD file|

### **II. GTKWAVE:** 

GTKWave is a VCD waveform viewer based on the GTK library. This viewer support VCD and LXT formats for signal dumps

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/My%20drawings/gtkwave.PNG)

Command to invoke waveform: `gtkwave veilog\_file.VCD`

### **III.  YOSYS**

Yosys is an open source RTL synthesis tool which takes verilog file of a design and technology library file(.lib) as inputs and generates gate level netlist corresponding the functional behaviour of the design

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/My%20drawings/yosys.PNG)

Table 2.3 Input files description
|**Input file/files**|`		`**Description**|
| :-: | :- |
|Verilog file|Contains functional description of a design in HDL|
|.lib file|Contains information about standard cell such area, power and timing, characterised for different PVT conditions We use open source library from SKY130 .|

Table 2.4 List of commands used for RTL Synthesis

|**Commands**|**Description**|
| :-: | :-: |
|`Yosys`|To invoke open source logic synthesizer|
|`read\_liberty -lib library\_name`|To read .lib file |
|`read\_verilog* verilog\_file.v`|To read verilog design file|
|`synth -top* verilog\_file`|To synthesize by setting a design as a top modules, if there are one or more submodules.|
|`abc -liberty* library\_name`|To generate gate level netlist|
|`show`|To display gate level schematic of the design|
|`write\_verilog*  verilog\_file\_netlist.v`|To write netlist into a verilog file.|

## **3. Flop coding style and optimization techniques**
Flops are introduced between the combinational circuits to avoid glitches in the circuit.

There are mainly three flop coding styles which are -

- Flop with asynchronous set/reset
- Flop with synchronous set/reset
- Flop with asynchronous and synchronous set/reset
### **Flop with asynchronous set/reset**
In this coding style asynchronous set/reset pin is not synchronized with the clock and it has got highest priority as compared to all other inputs. 
As shown in the following figure when reset is set to 1, irrespective of all other inputs output becomes zero.
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/DFF/DFF%20asyncres.PNG)
The following figure shows schematic of Flop with asynchronous reset. 
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/DFF/Synth_asyncres.PNG)
The verilog code is given by the following and it is importnant to observe the inputs which are present inside always statement. 
 <pre><code>module dff\_asyncres ( input clk ,  input async\_reset , input d , output reg q );
always @ (posedge clk , posedge async\_reset)
begin
    if(async\_reset)
      q <= 1'b0;
   else
     q <= d;
end </code></pre>

### **Flop with synchronous set/reset**
In this coding style asynchronous set/reset pin is synchronized with the clock and clock has got highest priority as compared to all other inputs. 
As shown in the following figure when reset is set to 1, irrespective of all other inputs output becomes zero during the next clock edge.
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/DFF/DFF_Syncres.PNG)
The following figure shows schematic of Flop with synchronous reset. 
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/DFF/Synth_sync_res.PNG)
The verilog code is given by the following and it is importnant to observe the inputs which are present inside always statement. 
 <pre><code>module dff\_syncres ( input clk , input async\_reset , input sync\_reset , input d , output reg q );
always @ (posedge clk )
begin
if (sync\_reset)
      q <= 1'b0;
else
      q <= d;
end
endmodule</code></pre>
### **Flop with synchronous and asynchronous set/reset**
In this coding style Din is synchronized with clock whereas asynchronous reset pit is not synchronized.
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/DFF/DFF_asyncres_syncres.PNG)
The following figure shows synthesis schematic of the design.
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/DFF/Synth_async_sync.PNG)
<pre><code>module dff\_asyncres\_syncres ( input clk , input async\_reset , input sync\_reset , input d , output reg q );
always @ (posedge clk , posedge async\_reset)
begin
   if(async\_reset)
      q <= 1'b0;
  else if (sync\_reset)
      q <= 1'b0;
  else	
      q <= d;
end
endmodule</code></pre>

