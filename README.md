# Exp-6b-Traffic-Light-Controller
# Aim
To design and simulate a Verilog HDL for Traffic Light Controler

# Apparatus Required
Vivado 2023.1
Spartan 7 FPGA
# Procedure
1. Launch Vivado 2023.1
Open Vivado 2023.1 and create a new project.
Select RTL Project and enable Do not specify sources at this time.
2. Create Verilog Design File
Create a new Verilog design file.
Type the Verilog code for the Traffic Light Controler

3. Observe the Output
Observe the Traffic Signal output.

# Block Diagram(Schematic)

<img width="1324" height="622" alt="image" src="https://github.com/user-attachments/assets/30f7dda7-edea-4f95-8c75-7207383ca65d" />

# code
```
module traffic_light_controller(
    input clk, rst,
    output reg [2:0] light  // {Red, Yellow, Green}
);

    // State encoding
  //  typedef enum reg [1:0] {RED=2'b00, GREEN=2'b01, YELLOW=2'b10} state_t;
 parameter [1:0]RED = 2'b00, GREEN=2'b01, YELLOW=2'b10 ;
 reg [1:0]state, next_state;  
 reg [3:0]count;
 // State register
    always @(posedge clk or posedge rst) begin
        if(rst) begin
            state <= RED;
            count <= 0;
        end
        else begin
            state <= next_state;
            count <= count + 1;
        end
    end
        always @(*) begin
        next_state = state;
        case(state)
            RED: if(count==4) // 4 cycles Red
                             next_state = GREEN; 
            GREEN:  if(count==6) next_state = YELLOW;  // 6 cycles Green
            YELLOW: if(count==2) next_state = RED;     // 2 cycles Yellow
        endcase
    end

    // Output logic
    always @(*) begin
        case(state)
            RED:    light = 3'b001; // Red ON
            GREEN:  light = 3'b010; // Green ON
            YELLOW: light = 3'b100; // Yellow ON
            default:light = 3'b000;
        endcase
    end

endmodule
```
# Test Bench

```
module tb_traffic_light;
    reg clk,rst;
    wire [2:0] light;

    traffic_light_controller uut(clk,rst,light);

    // Clock generation
    initial clk=0; always #5 clk=~clk;

    initial begin
        rst=1; #10; rst=0;
        #200 $finish;   // run simulation for 200 time units
    end

    initial begin
        $monitor("Time=%0t | Lights={Red,Yellow,Green}=%b", $time, light);
    end
endmodule
```
# output
<img width="1061" height="802" alt="image" src="https://github.com/user-attachments/assets/3ce37ea7-35c3-4188-96d0-1c0d63fa366a" />



# Result

The Verilog code for the Traffic Light Controller was designed and simulated successfully.
The output verified correct traffic light operation.


