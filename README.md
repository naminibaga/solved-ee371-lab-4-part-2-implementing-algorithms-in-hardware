Download Link: https://assignmentchef.com/product/solved-ee371-lab-4-part-2-implementing-algorithms-in-hardware
<br>
In this lab you will use algorithmic state machine charts to implement algorithms as hardware circuits.

Introduction

Algorithmic State Machine (ASM) charts are a design tool that allow the specification of digital systems in a form similar to a flow chart. An example of an ASM chart is shown in Figure 1. It represents a circuit that counts the number of bits set to 1 in an n-bit input A (<em>A</em>=<em>an</em><sub>−</sub>1<em>an</em><sub>−</sub>2…<em>a</em>1<em>a</em>0). The rectangular boxes in this diagram represent the <em>states</em> of the digital system, and actions specified inside of a state box occur on each active clock edge in this state. Transitions between states are specified by arrows. The diamonds in the ASM chart represent conditional tests, and the ovals represent actions taken only if the corresponding conditions are either true (on an arrow labeled 1) or false (on an arrow labeled 0).




Figure 1. ASM chart for a bit counting circuit

<img decoding="async" data-recalc-dims="1" data-src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/06/822.png?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/06/822.png?w=980&amp;ssl=1" data-recalc-dims="1">

 </noscript>

In this ASM chart, state <em>S</em>1 is the initial state. In this state the <em>result</em> is initialized to 0, and data is loaded into a register <em>A</em>, until a start signal, <em>s</em>, is asserted. The ASM chart then transitions to state <em>S</em>2, where it increments the <em>result</em> to count the number of 1’s in register <em>A</em>. Since state <em>S</em>2 specifies a shifting operation, then <em>A</em> should be implemented as a shift register. Also, since the <em>result</em> is incremented, then this variable should be implemented as a counter. When register <em>A</em> contains 0 the ASM chart transitions to state <em>S</em>3, where it sets an output <em>Done</em> =1 and waits for the signal <em>s</em> to be deasserted.

A key distinction between ASM charts and flow charts is a concept known as <em>implied timing</em>. The implied timing specifies that all actions associated with a given state take place only when the system is in that state when an active clock edge occurs. For example, when the system is in state <em>S</em>1 and the start signal <em>s</em> becomes 1, then the next active clock edge performs the following actions: initializes <em>result</em> to 0, and transitions to state <em>S</em>2. The action <em>right-shift A</em> does not happen yet, because the system is not yet in state <em>S</em>2. For each active clock cycle in state <em>S</em>2, the actions highlighted in Figure 1 take place, as follows: increment <em>result</em> if bit <em>a</em>0=1, change to state <em>S</em>3 if <em>A</em>=0 (or else remain in state <em>S</em>2), and shift <em>A</em> to the right.

The implementation of the bit counting circuit includes the counter to store the <em>result</em> and the shift register <em>A</em>, as well as a finite state machine. The FSM is often referred to as the <em>control</em> circuit, and the other components as the <em>datapath</em> circuit.

<h1>Task 1</h1>

Write SystemVerilog code to implement the bit-counting circuit using the ASM chart shown in Figure 1 on the DE1-SoC board. Include in your code the datapath components needed, and make an FSM for the control circuit. The inputs to your circuit should consist of an 8-bit input connected to slide switches <em>SW</em>7−0, a synchronous reset connected to <em>KEY</em>0, and a start signal (<em>s</em>) connected to switch <em>SW</em>9. Use the 50 MHz clock signal provided on the board as the clock input for your circuit. Be sure to synchronize the <em>s</em> signal to the clock. Display the number of 1s counted in the input data on the 7-segment display HEX0, and signal that the algorithm is finished by lighting up <em>LEDR</em>9.

<h1>Task 2</h1>

We wish to implement a binary search algorithm, which searches through an array to locate an 8-bit value <em>A</em> specified via switches <em>SW        </em>. A block diagram for the circuit is shown in Figure 2.

7−0




Figure 2. A block diagram for a circuit that performs a binary search.

The binary search algorithm works on a sorted array. Rather than comparing each value in the array to the one being sought, we first look at the middle element and compare the sought value to the middle element. If the middle element has a greater value, then we know that the element we seek must be in the first half of the array. Otherwise, the value we seek must be in the other half of the array. By applying this approach recursively, we can locate the sought element in only a few steps.

In this circuit, the array is stored in a memory module that is implemented inside the FPGA chip. A diagram of the memory module that we need to create is depicted in Figure 3. In a similar fashion to the first task of lab 2, create a memory that is eight-bits wide and 32 words deep.




Figure 3. The 32 x 8 RAM with address register

To place data into the memory, initialize the memory using the contents of a <em>memory initialization file (MIF)</em> and call it <em>my_array.mif</em>, which then has to be created in the folder that contains the Quartus project. Set the contents of your <em>MIF</em> file such that it contains a sorted collection of integers. Your circuit should produce a 5-bit output <em>L</em>, which specifies the address in the memory where the number <em>A</em> is located. In addition, a signal <em>Found</em> should be set high to indicate that the number <em>A</em> was found in the memory, and set low otherwise.

Perform the following steps:

<ol>

 <li>Create an ASM chart for the binary search algorithm. Keep in mind that the memory has registers on its input ports. Assume that the array has a fixed size of 32 elements.</li>

 <li>Implement the FSM and the datapath for your circuit.</li>

 <li>Connect your FSM and datapath to the memory block as indicated in Figure 2.</li>

 <li>Include in your project the necessary pin assignments to implement your circuit on your DE-series board. Use switch <em>SW</em>9 to drive the <em>Start</em> input, use <em>SW</em><sub>7…0</sub> to specify the value</li>

</ol>

<em>A</em>, use <em>KEY</em>0 for <em>Resetn</em>, and use the board’s 50 MHz clock signal as the <em>Clock</em> input (be sure to synchronize the <em>Start</em> input to the clock). Display the address of the data <em>A</em>, if found, on 7-segment displays <em>HEX</em>1 and <em>HEX</em>0, as a hexadecimal number. Finally, use <em>LEDR</em>9 for the <em>Found</em> signal.

<ol start="5">

 <li>Create a file called <em>mif</em> and fill it with an ordered set of 32 eight-bit integer numbers.</li>

 <li>Compile your design, and then download and test it.</li>

 <li>Use the SignalTap II functionality of Quartus to verify the contents of your RAM module. Find the Intel SignalTap II tutorial on canvas (SignalTap.pdf) to get started. For your RAM module, you’ll probably want to probe the data loaded via <em>mif</em>.</li>

 <li>Demonstrate to your TA.</li>

</ol>


