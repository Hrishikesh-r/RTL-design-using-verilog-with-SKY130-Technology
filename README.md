# RTL-design-using-verilog-with-SKY130-Technology
# Tools Used #
1. Iverilog(Icarus verilog)- A free compiler implementation for the IEEE-1364 Verilog hardware description language.
2. GTKWave-It is a VCD waveform viewer based on the GTK library. This viewer support VCD and LXT formats for signal dumpswer based on the GTK library.
3. Yosys - Yosys is an open-source synthesis tool. These are the open-source tools used in the labs for the workshop.
4. SKY130 - Sky130 process node and pdk(process design kit) are an open-source packages provided by Google and skywater for 130nm technology.
# Part-1
First step is to import all files including the sky130 libraries, We have Imported using git clone as shown below:

![git-clone](https://user-images.githubusercontent.com/91750776/165729380-2785b6b1-e0cf-4477-95b8-faa9c84337ed.jpg)

Once All files are Imported, We can proceed for simulation.
# Simulation Process 
The iverilog takes the verilog file and its test bench as input and produces a VCD (Value Change Dump) file. This VCD file can be viewed in GTKwave. Snippet below shows the terminal for simulation flow of a multiplexer:

![image](https://user-images.githubusercontent.com/91750776/165731908-d003d302-160a-4bfc-bb30-4c107124c832.png)

Waveform shown by the GTKwave for the multiplexer is:

![gtkwace-sim](https://user-images.githubusercontent.com/91750776/165732343-9d1b3ed5-5cbe-424b-8434-6cf17fc94e57.jpg)

# Synthesis Process
Yosys is the synthesis tool and needs to be Invoked as shown below:

![yosys](https://user-images.githubusercontent.com/91750776/165733102-d71bfad0-4167-467f-8f54-056530c59e3b.jpg)

We need Invoke the library file to the tool and this can be done by using the given below command:

![invoking libaray](https://user-images.githubusercontent.com/91750776/165733706-613ffe7b-3612-48dc-acd7-c292b535fe91.jpg)

Once the library is loaded, we need to give the verilog file as input by using the below command:

![reading verilog files](https://user-images.githubusercontent.com/91750776/165734170-27590565-acb6-4039-8e20-00c71b47f747.jpg)

We need to enter the below commands for converting to netlist:

![synthsesis top mod](https://user-images.githubusercontent.com/91750776/165734674-0fe12589-ee0f-4fc6-bfb7-0854df17cc4c.jpg)
![command to convert to netlist](https://user-images.githubusercontent.com/91750776/165734719-f9c2e1b7-8a88-4cf1-a3da-5642db264a1c.jpg)

Below Snippet shows the synthesis report of multiplexer:

![synthesis report](https://user-images.githubusercontent.com/91750776/165734844-3f22abc5-c9b1-40ac-a7ac-5349ff9e0bbd.jpg)

To view the generated netlist the command "write_verilog -noattr netlist_name.v", is used and to  view netlist in form of cells and their interconnection command "Show" is used.

![netlist_good mux](https://user-images.githubusercontent.com/91750776/165735108-95c2b5a5-2d6e-4f6c-a63c-1bbf48e5fb42.jpg)
![netlis](https://user-images.githubusercontent.com/91750776/165735336-1a4c735f-d658-48ac-8e37-133d0d47d4e2.jpg)

# Part-2
# $Library$
The library file used is sky130_fd_sc_hd_tt_025C_1v80.lib.The nomenclature indicates important parameters like Process,Temperature and Voltage.These parameters are shown in the "tt_025C_1v80".
1. tt means the process is "typical"
2. 025C shows the temperature - 25 degree centigrade
3. 1v80 shows the voltage - 1.8V

![library-Insite](https://user-images.githubusercontent.com/91750776/165736698-79fd7d26-9dcf-42e4-b63f-b4aeb6422a18.jpg)

The library contains different flavours of same logic gates. This is primarily done to meet the timing constraints.
1. Faster cells increases the clock frequency
-----T(clk) > T(cq_launch_flop) + T(combi) +T(setup_capture_flop)-----
2. Slower cells are required to prevent hold violations
-----T(hold_capture_flop) < T(cq_launch_flop) + T(combi)-----

# Flat and Hierarchial Synthesis

1. Hierarchial design has blocks, subblocks in an hierarchy; Flattened design has no subblocks and it has only leaf cells.
2. Hierarchical design takes more run time; Flattened design takes less run time.

Lets's consider the below example :
![image](https://user-images.githubusercontent.com/91750776/165770904-980a5ea3-f283-4535-b025-1b206d601c42.png)



if this logic is synthesised  hierarchically then, the netlist is as follows:

![heir-1_netlist](https://user-images.githubusercontent.com/91750776/165771026-7754a3e2-9ba8-4f9f-a1ac-9ee0c0c186c6.jpg)
![heir-2_netlist](https://user-images.githubusercontent.com/91750776/165771047-4aa6b2e1-2db4-40a5-9c50-9fff1dfeb940.jpg)

![Herirarchial synthesis](https://user-images.githubusercontent.com/91750776/165774570-27cb6634-1787-4435-8eea-65535d2b9655.jpg)


if this logic is synthesised  flat then, the netlist is as follows:

![flat-1_netlist](https://user-images.githubusercontent.com/91750776/165771203-f7fc1d48-9557-405f-878a-5e79bbd1f7f6.jpg)
![flat-2_netlist](https://user-images.githubusercontent.com/91750776/165771252-2dc505eb-61ce-4122-9301-71561324b190.jpg)

![Flat synthseis](https://user-images.githubusercontent.com/91750776/165774642-726be570-d543-46c5-abf9-262a08958aac.jpg)

Synthesis of Sudmodule1:

![submodule-1 synthesis](https://user-images.githubusercontent.com/91750776/165774083-4b16b5db-75bc-4c2d-a002-ca989674211d.jpg)


# Flip Flop Coding

Flip-flop or latch is a circuit that has two stable states and can be used to store state information. The circuit can be made to change state by signals applied to one or more control inputs and will have one or two outputs. Asynchronous design requires very careful attention to signal delays to avoid producing glitches and other spurious signals.Glitches will produce false data and can produce very wrong results.The use of memory and a clock [Flip-Flop] can eliminate signal races and glitches.
The set/reset logic is very important. They can be either synchronous or asynchronous. For flops with synchronous set/reset, the set/reset is dependant on clock and therefore this logic gets realised on the input (D- input).
The flip-flop can be asynchronous or synchronous depending on the sensitivity list.
1.  flipflop with asynchronous reset should have both clock and reset on the sensitiivity list.
2.  A flipflop with synchronous reset  havs only clock on the sensitivity list.
We need to use the command "dfflibmap -liberty ../path_to_library" in order to map to the FF available in the library.


[1] D Flip-Flop with Asynchronous reset[ Simulation and Netlist]

![asynres_dff](https://user-images.githubusercontent.com/91750776/165775162-14b984ac-6d26-41bd-b28c-087bae1a2581.jpg)
![asyncres_netlist](https://user-images.githubusercontent.com/91750776/165775202-92c00efd-6e9a-46c4-bd87-5bea8ee99a4b.jpg)

[2] D Flip-Flop with Asynchronous set[ Simulation and Netlist]

![asynset_dff](https://user-images.githubusercontent.com/91750776/165775381-b310fb4b-4274-4270-9207-b9bb1de93cd4.jpg)
![asyncset_netlist](https://user-images.githubusercontent.com/91750776/165775418-8e4229ae-4cc6-4eb3-8514-7b35acdfdeed.jpg)

[3] D Flip-Flop with synchronous reset[ Simulation and Netlist]

![synres_dff](https://user-images.githubusercontent.com/91750776/165775572-5f1daa2b-178e-4d84-b19d-daf03920f3ed.jpg)
![synres_netlist](https://user-images.githubusercontent.com/91750776/165775623-6b008d7c-2756-4253-a071-063836aef740.jpg)








