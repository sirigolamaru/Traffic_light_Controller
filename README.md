# Traffic_light_Controller


## AIM:

To design, implement, and simulate a Traffic Light Controller using Verilog HDL in Xilinx Vivado, controlling Red, Yellow, and Green lights at a road junction using an FSM.

## APPARATUS / TOOLS REQUIRED:

Vivado  tool(2024.2 or later recommended)

Computer / Laptop

Verilog HDL

## PROCEDURE: 

Open Vivado → Create New Project → RTL Project → No Default Part

Create New Verilog Module → traffic_light_controller.v

Define inputs (clk, reset) and outputs (red, yellow, green)

Declare FSM states: RED, GREEN, YELLOW

Use counter based on clock frequency to control timing

Implement state transition logic (Moore FSM)

Assign outputs based on state

Create testbench module to simulate different clock cycles and reset

Run Vivado Simulation → Check waveforms

Synthesize → Implement → Generate Bitstream (Optional for FPGA)

## VERILOG CODE FOR TRAFFIC LIGHT CONTROLLER:

       `timescale 1ns / 1ps
    module traffic_light_controller(
        input clk,
        input reset,
        output reg red,
        output reg yellow,
        output reg green
    );
    
    // State Encoding
    parameter GREEN  = 2'b00,
              YELLOW = 2'b01,
              RED    = 2'b10;
    
    // Timing counters
    parameter T_GREEN  = 10,
              T_YELLOW = 3,
              T_RED    = 7;
    
    reg [1:0] state, next_state;
    reg [3:0] timer; 
    
    // State Register
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= GREEN;
            timer <= 0;
        end
        else begin
        state <= next_state;
        timer <= timer + 1;
    end
    end
    
    // Next State Logic
    always @(*) begin
        case (state)
            GREEN: begin
                if (timer == T_GREEN) 
                    next_state = YELLOW;
                else 
                    next_state = GREEN;
            end

        YELLOW: begin
            if (timer == T_YELLOW)
                next_state = RED;
            else
                next_state = YELLOW;
        end

        RED: begin
            if (timer == T_RED)
                next_state = GREEN;
            else
                next_state = RED;
        end
        
        default: next_state = GREEN;
    endcase
    end

     // Output Logic
      always @(*) begin
    case(state)
        GREEN:  begin green=1; yellow=0; red=0; end
        YELLOW: begin green=0; yellow=1; red=0; end
        RED:    begin green=0; yellow=0; red=1; end
    endcase
     end

    // Reset timer on state change
    always @(posedge clk or posedge reset) begin
        if (reset)
            timer <= 0;
        else if (state != next_state)
            timer <= 0;
    end
    
    endmodule
## RTL DIAGRAM:
<img width="1366" height="768" alt="rtl diagram" src="https://github.com/user-attachments/assets/42d9b46a-528e-42e9-a534-8087b7e3d9eb" />



  ## TESTBENCH FOR TRAFFIC LIGHT CONTROLLER:

       `timescale 1ns / 1ps
          module tb;
          reg clk, reset;
          wire red, yellow, green;
          traffic_light_controller dut(clk, reset, red, yellow, green);
          
          // Clock
          always #5 clk = ~clk;  // 10ns period
          
          initial begin
              clk = 0; reset = 1;
              #20 reset = 0;
          
              #200 $finish;
          end
          
          endmodule

## OUTPUT WAVEFORM:
<img width="1366" height="768" alt="waveform" src="https://github.com/user-attachments/assets/c3d0bacb-b5ec-44f2-ac0d-64f2df4a7d71" />

CONCLUSION:<br>
The Traffic Light Controller project was successfully designed and implemented using Verilog HDL in Vivado. The design utilizes a Finite State Machine (FSM) to control the Red, Yellow, and Green traffic signals with accurate timing using a clock-based counter.

  
