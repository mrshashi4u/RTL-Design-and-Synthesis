# Table of contents
1. [**Day 1**:  Introduction to RTL design and Synthesis](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/README.md#1-introduction-to-rtl-design-and-synthesis)
	1. [Open Source tools for RTL Design and Synthesis](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#11-open-source-tools-for-rtl-design-and-synthesis)
		1. [iverilog](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#i--iverilog)
		2. [GTKWAVE](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#ii-gtkwave)
		3. [YOSYS](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#iii--yosys)
    
2. [**Day 2**: Introduction to .lib, Heirarchial vs flat synthesis and Flop coding style](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#3-flop-coding-style-and-optimization-techniques)
    1. [Introduction to .lib](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#31-introduction-to-lib)
    2. [Heirarchial vs flat synthesis](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#32-heirarchial-vs-flat-synthesis)
    3. [ Flop coding style](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#33-flop-coding-style)
    	1. [Flop with asynchronous set/reset](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#flop-with-asynchronous-setreset)
    	2. [Flop with synchronous set/reset](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#flop-with-synchronous-setreset)
    	3. [Flop with asynchronous and synchronous set/reset](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#flop-with-synchronous-and-asynchronous-setreset)
    	
3. [**Day 3**:  Introduction to optimisation](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#4-introduction-to-optimisation)
	1. [Optimization of combinational circuits](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#41-optimization-of-combinational-circuits)
		1. [Constant propagation](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#constant-propagation)
		2. [Optimization of boolean logic](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#optimization-of-boolean-logic)
	2.  [Optimization of sequential circuits](https://github.com/mrshashi4u/RTL-Design-and-Synthesis#42-optimization-of-sequential-circuits)
		1. [Optimization Sequential circuit using constant propagation](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/edit/main/README.md#optimization-sequential-circuit-using-constant-propagation)
	
# RTL-Design-and-Synthesis using opensource Skywater130 PDK

## **1. Introduction to RTL design and Synthesis**
Register Transfer Logic (RTL) is used to capture logic in design phase of the integrated circuit design cycle. A logic synthesis tool converts an RTL description written in Hardware Description Language (Verilog/VHDL) to a gate-level description of the circuit. Placement and routing tools use the synthesis outputs to generate a physical layout. The following figure shows the flow of RTL synthesis. 
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/My%20drawings/RTL%20design%20flow.PNG)

### **1.1. Open Source tools for RTL Design and Synthesis**

#### **I.  iverilog:** 
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

#### **II. GTKWAVE:** 

GTKWave is a VCD waveform viewer based on the GTK library. This viewer support VCD and LXT formats for signal dumps

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/My%20drawings/gtkwave.PNG)

Command to invoke waveform: `gtkwave veilog\_file.VCD`

#### **III.  YOSYS**

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

##**2. Introduction to .lib, Heirarchial vs flat synthesis and Flop coding style**

### **2.1. Introduction to .lib**

### **2.2. Heirarchial vs flat synthesis**

### **2.3. Flop coding style**
Flops are introduced between the combinational circuits to avoid glitches in the circuit.

There are mainly three flop coding styles which are -

- Flop with asynchronous set/reset
- Flop with synchronous set/reset
- Flop with asynchronous and synchronous set/reset
#### **Flop with asynchronous set/reset**
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

#### **Flop with synchronous set/reset**
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
#### **Flop with synchronous and asynchronous set/reset**
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

## **3. Introduction to optimisation**
Optimization in VLSI digital circuits is required in order to design an efficient circuit in terms of area and power.
### **3.1 Optimization of combinational circuits**
 In combinational circuit design optimization is done at two levels.
- Constant propagation
- Boolean logic optimization
#### **Constant propagation**
Let consider an the following combinational circuit: 

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/ex1_comb.PNG)


In the above circuit, if the input A=0, then the output is complement of C irrespective of the input B. Hence the entire circuit can be replaced by an inverter.
Let us look into somemore examples.

#### **Optimization of boolean logic**</p>
**Opt_Example 1:**
As an example, consider a 2:1 mux. A 2:1 mux requires two AND gates, one OR gate, and one inverter to be implemented. As a result, a total of four logic gates are required. 

Let us look into how optimization is performed with the following inputs.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Ex2_mux.PNG)

<pre><code>
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
</code></pre>

The synthesis of the above design shows that, the 2:1 mux is replaced with an OR gate and hence results in an optimization.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/opt_Check.PNG)

**Opt_Example 2:**

Let us consider the following mux based logic.
![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Opt_check2.PNG)

In the above logic circuit, the optimization leads to OR gate.

<pre><code>
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
</code></pre>

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Synt_opt_check2.PNG)

**Opt_Example 3:**

In the following example, two mux are connected to form a logic, and it requires total 8 logic gates to implement the mux.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Opt_check3.PNG)

In the above logic circuit, the optimization leads to three inputs AND gate.

<pre><code>
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
</code></pre>

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Synt_opt_check3.PNG)

**Opt_Example 4:**

In this example, two mux and two gates are connected to form a logic.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/opt_check4.PNG)

In the above logic circuit, the optimization leads top XNOR gate

<pre><code>
module opt_check4 (input a , input b , input c , output y);
 	assign y = a?(b?(a & c ):c):(!c);
 endmodule
</code></pre>

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Synt_opt_check4.PNG)

### **3.2 Optimization of sequential circuits**

- Constant Propagation
- Retiming 
- Cloning
- State optimization

We will restrict our discussion to sequential logic optimization in constant propagation.

**Sequential Example-1**

Consider a sequential circuit shown below

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Seq1.PNG) 

In the above circuit, if asynchronous reset is enabled then Q = 0 irrespective of clock, else Q =1 since D input is set to 1. 

The same can be verified in the simulated waveform shown below with verilog code.

<pre><code>
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
</pre></code>

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/seq1_wave.PNG)

The following figure shows the synthesis of the above circuit. As it can be seen in the above figure no optimization can be done with the design.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/seq1_synth.PNG)

**Sequential Example-2**

Consider an another sequential circuit shown below

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/seq2.PNG) 

In the above circuit, if the asynchronous set is enabled then Q = 1 and the output remains at the same state, since D input is set to 1. The same can be verified in the simulated waveform shown below along with verilog.

<pre><code>
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
</pre></code>


![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/seq2_wave.PNG)

As shown in the wave form, the output remains at logic 1, irrespective of states of the other inputs. Hence the above design can be replaced by a simple buffer there by optimizing power and area.

The following figure shows the synthesis of the above circuit. As it can be seen in the synthesis design, optimization is performed by in synthesis stage by replacing the entire design by a simple wire buffer.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/synt_seq2.PNG)

**Sequential Example-3**

Consider an another sequential circuit shown below

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/seq3.PNG) 
The above circuit consists of two asynchronous set and reset DFF. Hence the optimization can't be done in the above design. 


The following figure shows the synthesis of the above circuit along with verilog code. As it can be seen in the synthesis design, optimization is performed  in synthesis stage by replacing the entire design by a simple wire buffer.

<pre><code>
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
</pre></code>

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/synt_seq3.PNG)

**Sequential Example-4**

In the design sequential example - 3, the optimization can be achieved by replacing asynchronous **reset DFF** by asynchronous **set DFF** which is as shown in the below example.

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/seq4.PNG) 


The above circuit consists of two asynchronous set DFF's. Hence the output remains at logic irreseprctive of clock. Hence optimization in the above design results in a buffer. 


The following figure shows the synthesis of the above circuit along with the code. As it can be seen in the synthesis design, the two flipflops are replaced by two buffers.

<pre><code>
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
</pre></code>


![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/synt_seq4.PNG)

**Sequential optimization of unused outputs**

Let us consider following verilog code of an up counter.

<pre><code>

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
			
</pre></code>

In the above example, the output is assigned to LSB of counter value. Hence we can optimize the design for the required output so that additional hardware overhead can be minimized.
			
The following figure show the synthesized output of the above logic. 

![](https://github.com/mrshashi4u/RTL-Design-and-Synthesis/blob/main/D3/Counter_opt.PNG)

As shown in the above figure, only one DFF is utilized in the design in contrast to 3-DFF's for a 3-bit counter. Hence the optimization results in reduced area and power.


