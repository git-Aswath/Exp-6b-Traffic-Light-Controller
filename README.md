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

# code
module traffic_light_controller(
    input clk, 
    input rst,
    output reg [2:0] light
);

    
    parameter [1:0] RED = 2'b00, 
                    GREEN = 2'b01, 
                    YELLOW = 2'b10;

    reg [1:0] state, next_state;
    reg [3:0] count;

   
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            state <= RED;
            count <= 0;
        end else begin
            state <= next_state;
            
            
            if (state != next_state)
                count <= 0;
            else
                count <= count + 1;
        end
    end

    
    always @(*) begin
        next_state = state; 

        case (state)
            RED: begin
                if (count == 4) 
                    next_state = GREEN;
            end

            GREEN: begin
                if (count == 6) 
                    next_state = YELLOW;
            end

            YELLOW: begin
                if (count == 2) 
                    next_state = RED;
            end

            default: next_state = RED;
        endcase
    end

   
    always @(*) begin
        case (state)
            RED:    light = 3'b001; 
            GREEN:  light = 3'b010; 
            YELLOW: light = 3'b100; 
            default:light = 3'b000;
        endcase
    end
endmodule

# Tes Bench
module tb_traffic_light;
    reg clk,rst;
    wire [2:0] light;

    traffic_light_controller uut(clk,rst,light);

    
    initial clk=0; always #5 clk=~clk;

    initial begin
        rst=1; #10; rst=0;
        #200 $finish;   
    end

    initial begin
        $monitor("Time=%0t | Lights={Red,Yellow,Green}=%b", $time, light);
    end
endmodule

# output

<img width="1914" height="1196" alt="Screenshot 2025-10-24 154019" src="https://github.com/user-attachments/assets/be3bb065-4fa4-4a17-b966-4d87bc15ece6" />

# Result




