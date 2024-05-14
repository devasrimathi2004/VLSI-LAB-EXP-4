# VLSI-LAB-EXP-4
SIMULATION AND IMPLEMENTATION OF SEQUENTIAL LOGIC CIRCUITS

AIM: 
 To simulate and synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN using Xilinx ISE.

APPARATUS REQUIRED:

Xilinx 14.7
Spartan6 FPGA

**LOGIC DIAGRAM**

SR FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/77fb7f38-5649-4778-a987-8468df9ea3c3)


JK FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/1510e030-4ddc-42b1-88ce-d00f6f0dc7e6)

T FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/7a020379-efb1-4104-85ee-439d660baa08)


D FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/dda843c5-f0a0-4b51-93a2-eaa4b7fa8aa0)


COUNTER

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/a1fc5f68-aafb-49a1-93d2-779529f525fa)


  
PROCEDURE:
STEP:1  Start  the Xilinx navigator, Select and Name the New project.
STEP:2  Select the device family, device, package and speed.       
STEP:3  Select new source in the New Project and select Verilog Module as the Source type.                       
STEP:4  Type the File Name and Click Next and then finish button. Type the code and save it.
STEP:5  Select the Behavioral Simulation in the Source Window and click the check syntax.                       
STEP:6  Click the simulation to simulate the program and  give the inputs and verify the outputs as per the truth table.               
STEP:7  Select the Implementation in the Sources Window and select the required file in the Processes Window.
STEP:8  Select Check Syntax from the Synthesize  XST Process. Double Click in the  FloorplanArea/IO/Logic-Post Synthesis process in the User Constraints process group. UCF(User constraint File) is obtained. 
STEP:9  In the Design Object List Window, enter the pin location for each pin in the Loc column Select save from the File menu.
STEP:10 Double click on the Implement Design and double click on the Generate Programming File to create a bitstream of the design.(.v) file is converted into .bit file here.
STEP:11  On the board, by giving required input, the LEDs starts to glow light, indicating the output.

VERILOG CODE:

D_FLIPFLOP:
~~~
module dff(d,clk,rst,q);
input d,clk,rst;
output reg q;
always @(posedge clk)
begin
if (rst==1)
q=1'b0;
else
q=d;
end
endmodule
~~~

OUTPUT:

![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/76d68d9b-e438-42b2-a835-e23b4fce35c0)
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/95f9c712-c728-4abf-b7b9-e62e4f4ed394)


JK_FLIPFLOP:
~~~
module JK_flipflop (q, q_bar, j,k, clk, reset);  
  input j,k,clk, reset;
  output reg q;
  output q_bar;
  // always@(posedge clk or negedge reset) // for asynchronous reset
  always@(posedge clk) begin // for synchronous reset
    if(!reset)
           q <= 0;
    else 
  begin
      case({j,k})
        2'b00: q <= q;    // No change
        2'b01: q <= 1'b0; // reset
        2'b10: q <= 1'b1; // set
        2'b11: q <= ~q; // Toggle
      endcase
    end
  end
  assign q_bar = ~q;
endmodule
~~~

OUTPUT:

![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/36f309da-c0d6-438d-ada1-0954aa51791e)
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/b089fb49-f93b-4d00-acfe-66dd525b7a7b)


MOD_10_COUNTER:
~~~
module counter(
input clk,rst,enable,
output reg [3:0]counter_output
);
always@ (posedge clk)
begin 
if( rst | counter_output==4'b1001)
counter_output <= 4'b0000;
else if(enable)
counter_output <= counter_output + 1;
else
counter_output <= 0;
end
endmodule
~~~

OUTPUT:

![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/9031cd2e-ae93-421f-a98f-9dbf66e86a06)
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/6789bbc4-1ed8-4a97-9012-bcba183d81df)


RIPPLECARRYCOUNTER:
~~~
module D_FF(q, d, clk, reset);
output q;
input d, clk, reset;
reg q;
always @(posedge reset or negedge clk)
if (reset)
q = 1'b0;
else
q = d;
endmodule
module T_FF(q, clk, reset);
output q;
input clk, reset;
wire d;
D_FF dff0(q, d, clk, reset);
not n1(d, q); 
endmodule
module ripple_carry_counter(q, clk, reset);
output [3:0] q;
input clk, reset;
T_FF tffo(q[0], clk, reset);
T_FF tff1(q[1], q[0], reset);
T_FF tff2(q[2], q[1], reset);
T_FF tff3(q[3], q[2], reset);
endmodule
~~~

OUTPUT:

![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/7eb7f21b-0615-4148-af32-6e9c66a613c9)
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/020f4f6f-d60b-46da-b341-75054d6ff3e5)


SR_FLIPFLOP:
~~~
module SR_flipflop (q, q_bar, s,r, clk, reset);
  input s,r,clk, reset;
  output reg q;
  output q_bar;
  always@(posedge clk) begin 
    if(!reset)�        q <= 0;
    else 
  begin
      case({s,r})
        2'b00: q <= q;    
        2'b01: q <= 1'b0; 
        2'b10: q <= 1'b1; 
        2'b11: q <= 1'bx; 
      endcase
    end
  end
  assign q_bar = ~q;
endmodule
~~~

OUTPUT:

![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/1072f464-27ba-4309-a03c-2f69bde6470e)
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/74f60477-c190-494b-81db-c2874894ab1e)


TFLIPFLOP:
~~~
module tff (t,clk, rstn,q);  
 input t,clk, rstn;
 output reg q;
  always @ (posedge clk) begin  
    if (!rstn)  
      q <= 0;  
    else  
        if (t)  
            q <= ~q;  
        else  
            q <= q;  
  end  
endmodule
~~~

OUTPUT:
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/5bad431b-0e94-4673-b3c5-f9165f0b7344)
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/76fd755e-4c50-465f-ab99-16531813e2c6)


UPDOWNCOUNTER:
~~~
module updown_counter(clk,rst,updown,out);
input clk,rst,updown;
output reg [3:0]out;
always@(posedge clk)
begin
if (rst==1)
out=4'b0000;
else if(updown==1)
out=out+1;
else
out=out-1;
end
endmodule
~~~

OUTPUT:
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/5d6aacfc-e370-4c4f-9ab0-c7c16ed44c3b)
![image](https://github.com/devasrimathi2004/VLSI-LAB-EXP-4/assets/166363441/bcc19154-e488-42f8-be36-b1db84d0dcca)



RESULT:Simulate And Synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN is Successfully Verified using Vivado Software.


