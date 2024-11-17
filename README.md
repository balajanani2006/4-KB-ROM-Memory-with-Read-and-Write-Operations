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

module ripple_adders ( input [3:0] A, input [3:0] B, input Cin, output [3:0] Sum, output Cout );

reg [3:0] sum_temp;
reg cout_temp;
reg cout_final;

task full_adder;
    input a, b, cin;
    output sum, cout;
begin
    sum = a ^ b ^ cin;
    cout = (a & b) | (b & cin) | (cin & a);
end
endtask

always @(*) begin
    full_adder(A[0], B[0], Cin, sum_temp[0], cout_temp);
    full_adder(A[1], B[1], cout_temp, sum_temp[1], cout_temp);
    full_adder(A[2], B[2], cout_temp, sum_temp[2], cout_temp);
    full_adder(A[3], B[3], cout_temp, sum_temp[3], cout_final);
end

assign Sum = sum_temp;
assign Cout = cout_final;
endmodule

Testbench code for 4-bit Ripple carry adder
module ripple_adder_tb;

reg [3:0] A, B;
reg Cin;
wire [3:0] Sum;
wire Cout;

// Instantiate the ripple carry adder
ripple_adders uut (
    .A(A),
    .B(B),
    .Cin(Cin),
    .Sum(Sum),
    .Cout(Cout)
);

initial begin
    // Test cases
    A = 4'b0001; B = 4'b0010; Cin = 0; #10;
    A = 4'b0110; B = 4'b0101; Cin = 0; #10;
    A = 4'b1111; B = 4'b0001; Cin = 0; #10;
    A = 4'b1010; B = 4'b1101; Cin = 1; #10;
    A = 4'b1111; B = 4'b1111; Cin = 1; #10;
    $stop;
end

initial begin
    $monitor("Time = %0t | A = %b | B = %b | Cin = %b | Sum = %b | Cout = %b",
             $time, A, B, Cin, Sum, Cout);
end

![image](https://github.com/user-attachments/assets/34be9acd-9331-484b-abb9-689c95d3ad7d)


Verilog code for 4-bit Ripple counter
module ripple_counter_4bit ( input clk, // Clock signal input reset, // Reset signal output reg [3:0] Q // 4-bit output for the counter value );

// Function to calculate next state function [3:0] next_state; input [3:0] curr_state; begin next_state = curr_state + 1; end endfunction

// Sequential logic for counter always @(posedge clk or posedge reset) begin if (reset) Q <= 4'b0000; // Reset the counter to 0 else Q <= next_state(Q); // Increment the counter end

endmodule

Testbench code for 4-bit Ripple counter
module ripple_counter_4bit_tb;

reg clk; reg reset; wire [3:0] Q;

// Instantiate the ripple counter ripple_counter_4bit uut ( .clk(clk), .reset(reset), .Q(Q) );

// Clock generation (10ns period) always #5 clk = ~clk;

initial begin // Initialize inputs clk = 0; reset = 1;

// Hold reset for 20ns
#20 reset = 0;

// Run simulation for 200ns
#200 $stop;
end

initial begin $monitor("Time = %0t | Reset = %b | Q = %b", $time, reset, Q); end

endmodule

OUTPUT

![image](https://github.com/user-attachments/assets/058635da-b9f0-4455-a4e9-ff778d3ff312)








Conclusion
In this experiment, a 4KB ROM memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes for ROM.
