# Lab 03 Report

Name: Uma Akundi
Email: uakun001@ucr.edu

## Implementation

In this lab, I implemented the main control unit for a simple MIPS datapath. The control unit takes in the opcode from the instruction and sets the correct datapath control signals for each supported instruction type.
I completed the control.v file by adding a case statement inside the given always @(instr_op) block.

For R-type instructions, the control unit sets alu_op = 2'b10 so that the ALU control unit uses the funct field to decide the ALU operation.

For lw and sw, the ALU needs to calculate an address, so I used alu_op = 2'b00 for add. For beq, the ALU subtracts the two register values, so I used alu_op = 2'b01.

For addi, I filled in the missing immediate instruction control signals. Since addi writes the ALU result back into a register and uses an immediate value, I used:

verilog
reg_dst    = 1'b0;
branch     = 1'b0;
mem_read   = 1'b0;
mem_to_reg = 1'b0;
alu_op     = 2'b00;
mem_write  = 1'b0;
alu_src    = 1'b1;
reg_write  = 1'b1;


## Test Cases

I added the provided test cases to lab03_tb.v. The testbench checks the outputs of the control unit, ALU control unit, and ALU.

The test cases included:

* R-type AND
* R-type OR
* R-type ADD
* R-type SUB
* R-type SLT
* R-type NOR
* addi
* lw
* sw
* beq

The addi test case was important because the lab required filling in the missing control values for the immediate instruction.

I compiled the Verilog files with:
bash
iverilog -o lab03_tb.out lab03_tb.v control.v alu_control.v alu.v

Then I ran the testbench with
bash
vvp lab03_tb.out
## Results
VCD info: dumpfile lab03.vcd opened for output.
        Test Case 1: R-type (add) ...passed
        Test Case 2: R-type (or) ...passed
        Test Case 3: R-type (add) ...passed
        Test Case 4: R-type (sub) ...passed
        Test Case 5: R-type (slt) ...passed
        Test Case 6: R-type (nor) ...passed
        Test Case 7: addi ...passed
        Test Case 8: lw ...passed
        Test Case 9: sw ...passed
        Test Case 10: beq ...passed



## Conclusion

The main thing I had to make sure of was that the correct control signals were used for addi. Since addi is not exactly the same as R-type or lw, I had to trace what the datapath should do.

For addi, the ALU should add a register value and an immediate value. This means alu_src should be set to 1, and alu_op should be 00 for add. Since the result is written back to a register reg_write should be 1. Since memory is not used, both mem_read and mem_write should be 0.

This lab helped me understand how the main control unit connects the instruction opcode to the rest of the datapath. Each instruction type needs a different combination of control signals, even when some instructions use the ALU in similar ways. 
