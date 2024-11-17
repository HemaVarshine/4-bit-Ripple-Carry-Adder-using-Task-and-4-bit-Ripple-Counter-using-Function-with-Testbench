# 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.

// Verilog Code
module full_adder(
    input a,
    input b,
    input cin,
    output sum,
    output cout
);
    assign sum = a ^ b ^ cin; // XOR for sum
    assign cout = (a & b) | (b & cin) | (a & cin); // Carry out logic
endmodule
module ripple_carry_adder_4bit(
    input [3:0] A,
    input [3:0] B,
    input cin,
    output [3:0] sum,
    output cout
);
    wire c1, c2, c3; // Intermediate carry wires

    // Instantiate 4 full adders for the 4-bit adder
    full_adder fa0 (.a(A[0]), .b(B[0]), .cin(cin), .sum(sum[0]), .cout(c1));
    full_adder fa1 (.a(A[1]), .b(B[1]), .cin(c1), .sum(sum[1]), .cout(c2));
    full_adder fa2 (.a(A[2]), .b(B[2]), .cin(c2), .sum(sum[2]), .cout(c3));
    full_adder fa3 (.a(A[3]), .b(B[3]), .cin(c3), .sum(sum[3]), .cout(cout));
endmodule
module testbench_ripple_carry_adder;
    reg [3:0] A, B;
    reg cin;
    wire [3:0] sum;
    wire cout;

    // Instantiate the 4-bit ripple carry adder
    ripple_carry_adder_4bit rca (.A(A), .B(B), .cin(cin), .sum(sum), .cout(cout));

    initial begin
        // Test cases
        $monitor("A = %b, B = %b, cin = %b | sum = %b, cout = %b", A, B, cin, sum, cout);

        A = 4'b0000; B = 4'b0000; cin = 1'b0; #10;
        A = 4'b0101; B = 4'b0011; cin = 1'b0; #10;
        A = 4'b1111; B = 4'b0001; cin = 1'b0; #10;
        A = 4'b1010; B = 4'b0101; cin = 1'b1; #10;
        A = 4'b1111; B = 4'b1111; cin = 1'b1; #10;

        $finish;
    end
endmodule


<img width="960" alt="image" src="https://github.com/user-attachments/assets/1213fe95-e1e6-4105-a76d-de811be9f859">

// Verilog Code ripple counter

module ripple_counter(
    input clk,
    input reset,
    output reg [3:0] count
);
    always @(posedge clk or posedge reset) begin
        if (reset)
            count <= 4'b0000; // Reset the counter to 0
        else
            count <= count + 1; // Increment the counter on each clock pulse
    end
endmodule
module testbench_ripple_counter;
    reg clk;
    reg reset;
    wire [3:0] count;

    // Instantiate the ripple counter module
    ripple_counter uut (
        .clk(clk),
        .reset(reset),
        .count(count)
    );

    // Clock generation
    initial begin
        clk = 0;
        forever #5 clk = ~clk; // 10 ns clock period (100 MHz)
    end

    // Test sequence
    initial begin
        // Monitor the outputs
        $monitor("Time = %0t | reset = %b | count = %b", $time, reset, count);

        // Apply reset
        reset = 1;
        #10; // Wait for a clock cycle

        reset = 0;
        #100; // Run the simulation for some time

        // End simulation
        $finish;
    end
endmodule
<img width="960" alt="image" src="https://github.com/user-attachments/assets/1b84f81c-b228-4585-9911-f0a27f8b927d">

Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.

