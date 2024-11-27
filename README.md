module tb_chocolate_vending_machine;

    // Declare inputs as reg type (for stimulus generation)
    reg clk;
    reg reset;
    reg nickel;
    reg dime;
    reg quarter;
    reg chocolate_select_1;
    reg chocolate_select_2;
    reg chocolate_select_3;

    // Declare outputs as wire type (for observing outputs)
    wire chocolate_dispensed;
    wire change_returned;
    wire [7:0] balance;

    // Instantiate the chocolate vending machine module
    chocolate_vending_machine uut (
        .clk(clk),
        .reset(reset),
        .nickel(nickel),
        .dime(dime),
        .quarter(quarter),
        .chocolate_select_1(chocolate_select_1),
        .chocolate_select_2(chocolate_select_2),
        .chocolate_select_3(chocolate_select_3),
        .chocolate_dispensed(chocolate_dispensed),
        .change_returned(change_returned),
        .balance(balance)
    );

    // Clock generation: 10ns period (100MHz clock)
    always begin
        #5 clk = ~clk;
    end

    // Stimulus generation
    initial begin
        // Initialize all signals
        clk = 0;
        reset = 0;
        nickel = 0;
        dime = 0;
        quarter = 0;
        chocolate_select_1 = 0;
        chocolate_select_2 = 0;
        chocolate_select_3 = 0;

        // Apply reset to initialize the machine
        reset = 1;
        #10 reset = 0;
        
        // Test Case 1: Insert coins and select chocolate 1 (price = 50 cents)
        // Insert a nickel (5 cents)
        nickel = 1; 
        #10 nickel = 0; // Reset nickel input

        // Insert a dime (10 cents)
        dime = 1; 
        #10 dime = 0;  // Reset dime input

        // Insert a quarter (25 cents)
        quarter = 1; 
        #10 quarter = 0; // Reset quarter input

        // Select chocolate 1 (price = 50 cents)
        chocolate_select_1 = 1;
        #10 chocolate_select_1 = 0;

        // Wait for the chocolate to be dispensed
        #10;
        
        // Check if chocolate was dispensed and change returned
        $display("Test Case 1: Dispensed = %b, Change Returned = %b, Balance = %d", chocolate_dispensed, change_returned, balance);

        // End simulation
        $stop;
    end

endmodule
