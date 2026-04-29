`timescale 1ns / 1ps

module smart_traffic_controller(
    input clk,
    input reset,

    input north_sensor,
    input east_sensor,
    input south_sensor,
    input west_sensor,

    input emergency_vehicle,     
    input pedestrian_request,
    input system_fault,

    input [1:0] time_of_day,

    output reg [2:0] N,
    output reg [2:0] E,
    output reg [2:0] S,
    output reg [2:0] W,

    output reg pedestrian_light,
    output reg emergency_mode,
    output reg system_healthy
);

////////////////////////////////////////////////////////////
// Light Encoding
////////////////////////////////////////////////////////////

parameter RED    = 3'b100;
parameter YELLOW = 3'b010;
parameter GREEN  = 3'b001;

////////////////////////////////////////////////////////////
// Timing Parameters
////////////////////////////////////////////////////////////

parameter GREEN_DAY   = 10;
parameter GREEN_PEAK  = 15;
parameter GREEN_NIGHT = 6;
parameter YELLOW_TIME = 3;
parameter PED_TIME    = 5;
parameter SAFE_TIME   = 2;
parameter EMERGENCY_TIME = 6;

////////////////////////////////////////////////////////////
// FSM States
////////////////////////////////////////////////////////////

parameter S0  = 4'd0;
parameter S1  = 4'd1;
parameter S2  = 4'd2;
parameter S3  = 4'd3;
parameter S4  = 4'd4;
parameter S5  = 4'd5;
parameter S6  = 4'd6;
parameter S7  = 4'd7;
parameter S8  = 4'd8;
parameter S9  = 4'd9;
parameter S10 = 4'd10;

reg [3:0] current_state;
reg [3:0] next_state;

reg [7:0] counter;
reg [7:0] green_time;

////////////////////////////////////////////////////////////
// Adaptive Timing
////////////////////////////////////////////////////////////

always @(*) begin
    case(time_of_day)
        2'b00: green_time = GREEN_NIGHT;
        2'b01: green_time = GREEN_DAY;
        2'b10: green_time = GREEN_PEAK;
        default: green_time = GREEN_DAY;
    endcase
end

////////////////////////////////////////////////////////////
// Sequential Logic
////////////////////////////////////////////////////////////

always @(posedge clk or posedge reset) begin
    if (reset) begin
        current_state <= S0;
        counter <= 0;
        system_healthy <= 1'b1;
    end
    else begin
        current_state <= next_state;

        if(system_fault)
            system_healthy <= 1'b0;
        else
            system_healthy <= 1'b1;

        if(current_state != next_state)
            counter <= 0;
        else
            counter <= counter + 1;
    end
end

////////////////////////////////////////////////////////////
// Next State Logic
////////////////////////////////////////////////////////////

always @(*) begin
    next_state = current_state;

    if(emergency_vehicle)
        next_state = S10;

    else if(system_fault)
        next_state = S9;

    else begin
        case(current_state)

            S0: if(counter >= green_time) next_state = S1;

            S1: begin
                if(counter >= YELLOW_TIME) begin
                    if(pedestrian_request)
                        next_state = S8;
                    else
                        next_state = S2;
                end
            end

            S2: if(counter >= green_time) next_state = S3;
            S3: if(counter >= YELLOW_TIME) next_state = S4;

            S4: if(counter >= green_time) next_state = S5;
            S5: if(counter >= YELLOW_TIME) next_state = S6;

            S6: if(counter >= green_time) next_state = S7;
            S7: if(counter >= YELLOW_TIME) next_state = S0;

            S8: if(counter >= PED_TIME) next_state = S9;
            S9: if(counter >= SAFE_TIME) next_state = S0;
            S10: if(counter >= EMERGENCY_TIME) next_state = S0;

            default: next_state = S0;
        endcase
    end
end

////////////////////////////////////////////////////////////
// Output Logic
////////////////////////////////////////////////////////////

always @(*) begin

    N = RED;
    E = RED;
    S = RED;
    W = RED;

    pedestrian_light = 1'b0;
    emergency_mode = 1'b0;

    case(current_state)

        S0: N = GREEN;
        S1: N = YELLOW;

        S2: E = GREEN;
        S3: E = YELLOW;

        S4: S = GREEN;
        S5: S = YELLOW;

        S6: W = GREEN;
        S7: W = YELLOW;

        S8: pedestrian_light = 1'b1;

        S10: begin
            N = YELLOW;
            emergency_mode = 1'b1;
        end

    endcase
end

endmodule
