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
|<p>**Verilog file**</p><p></p>|<p>Contains functional description of a design in HDL. **Ex: 2:1 Mux verilog file.n**</p><p></p><p>*module m21(i0, i1,sel, y);*</p><p>*output y;*</p><p>*input i0, i1, sel;*</p><p></p><p>*assign y=(sel)?i1:i0;*</p><p></p><p>*endmodule*</p><p></p>|
