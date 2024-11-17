# 4 KB-ROM-Memory-with-Read-and-Write-Operations
Aim
To design and simulate a 4KB ROM memory with read and write operations using Verilog HDL and verify the functionality through a testbench in the Vivado 2023.1 simulation environment.

Apparatus Required
Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.
Procedure
Launch Vivado 2023.1:

Open Vivado and create a new project.
Design the Verilog Code for ROM:

Write the Verilog code for a 4KB ROM memory with read and write capabilities.
Create the Testbench:

Write a testbench to simulate both the read and write operations, verifying that the data is correctly written to and read from the memory.
Add the Verilog Files:

Add the ROM Verilog module and the testbench file to the project.
Run Simulation:

Run the behavioral simulation in Vivado and check the memory's read and write operations.
Observe the Waveforms:

Analyze the waveform to verify that the memory read and write operations work as expected.
Save and Document Results:

Capture the waveform and include the simulation results in the final report.
Verilog Code for 4KB ROM Memory with Read and Write Operations
In this design, we will implement a 4KB ROM. Since ROM is typically read-only, we will simulate the behavior as if it's writable, but in actual hardware, ROM is typically pre-programmed.

4KB = 4096 Bytes = 4096 x 8 bits
```
module rom_memory ( input clk,
input rst,
input rw,
input [11:0] address,
input [7:0] data_in,
output reg [7:0] data_out
); reg [7:0] mem[0:4095]; always @(posedge clk) begin if (rst) begin data_out <= 8'd0;
end else if (rw) begin mem[address] <= data_in; end else begin data_out <= mem[address]; end end

endmodule

```

![image](https://github.com/user-attachments/assets/7c9e7eac-ffea-4ec5-8f08-b4284fdd2a02)


Testbench for 4KB ROM Memory

`timescale 1ns / 1ps
```
module tb_rom_memory; reg clk; reg rst; reg rw;
reg [11:0] address;
reg [7:0] data_in;
wire [7:0] data_out;
rom_memory uut ( .clk(clk), .rst(rst), .rw(rw), .address(address), .data_in(data_in), .data_out(data_out) ); initial begin clk = 0; forever #5 clk = ~clk;
end initial begin rst = 1; rw = 0; address = 12'd0; data_in = 8'd0; #10; rst = 0; #10; rw = 1;
address = 12'd10;
data_in = 8'd55;
#10; address = 12'd20;
data_in = 8'd100;
#10; address = 12'd30;
data_in = 8'd200;
#10; rw = 0;
#10; address = 12'd10; #10; $display("Read from Address 10: %d", data_out); #10; address = 12'd20; #10; $display("Read from Address 20: %d", data_out); #10; address = 12'd30; #10; $display("Read from Address 30: %d", data_out);
#10; $finish; end

endmodule
```

OUTPUT

![image](https://github.com/user-attachments/assets/db0ad87a-fff2-48b0-963b-cfec3c201c67)

Conclusion
In this experiment, a 4KB ROM memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes for ROM.





