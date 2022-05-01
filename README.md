![image](https://user-images.githubusercontent.com/91750776/166153540-b4db9121-9da6-44c8-939d-999a75f047fa.png)


# Contents 

Part-1

part-2

part-3

part-4

part-5 


# RTL-design-using-verilog-with-SKY130-Technology
# Tools Used #
1. Iverilog(Icarus verilog)- A free compiler implementation for the IEEE-1364 Verilog hardware description language.
2. GTKWave-It is a VCD waveform viewer based on the GTK library. This viewer support VCD and LXT formats for signal dumpswer based on the GTK library.
3. Yosys - Yosys is an open-source synthesis tool. These are the open-source tools used in the labs for the workshop.

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

Synthesis of Submodule1:

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


# Part-3

# Optimization

What is Optimization ?

Optimization is the process of iterating through a design such that it meets timing, area and power specifications. 

What are the types of Optimization in Combinational Circuits?

1. Constant Propagation- Here, constant inputs to the logic are propagated to the output which results in a minimized expression implementing the same logic.
2. Boolean logic optimization- This a techniques of minimizing large boolean expression using laws of boolean algebra.

we  use the command "otp_clean -purge" to optimize the design and remove unwanted components after running synth command.

![opt_clean_prge](https://user-images.githubusercontent.com/91750776/165957777-b7c85968-1a4e-451b-a892-7f0347b8ed8c.jpg)


Below are the few examples of Combinational optimization with their Netlist:

Example 1 :

![opt_check_code](https://user-images.githubusercontent.com/91750776/165955579-9215f538-c124-40e7-9753-8558ce1ca04c.jpg)![opt_check_netlist](https://user-images.githubusercontent.com/91750776/165956364-a3a147e4-35f5-4200-b46b-f875c9add10c.jpg)

Example 2 :

![opt_check2_code](https://user-images.githubusercontent.com/91750776/165957135-4bef189e-5a69-4f47-911d-072bcc58a529.jpg)
![opt_check2_netlist](https://user-images.githubusercontent.com/91750776/165957180-a302944b-e82d-4f05-9376-788d555881ab.jpg)

Example 3 :

![opt_check3_code](https://user-images.githubusercontent.com/91750776/165957598-6a1de0f9-db64-42d6-b1bf-08c090c192aa.jpg)
![opt_check3_netlist](https://user-images.githubusercontent.com/91750776/165957634-0896f9d3-befd-44e7-bb82-d1ea88e42e6e.jpg)

Example 4 :

![opt_check4_ccode](https://user-images.githubusercontent.com/91750776/165957712-f9a63c6a-1822-4d50-a23f-8699ac4cee0c.jpg)
![opt_check4_netlist](https://user-images.githubusercontent.com/91750776/165957738-9c8a74e5-1a51-49ea-94a2-b9f9d2053aa3.jpg)

List the types of Sequential Optimizations.

Sequential logic optimization can be divided into two

1. Basic

-> Sequential constant propagation

2. Advanced

-> State optimization

-> Retiming

-> Sequential logic cloning (floor plan aware synthesis)


Below are the few examples of Sequential optimization with their Netlist, waveform :

Example 1 :

![dff_const1_code](https://user-images.githubusercontent.com/91750776/165960989-b65801d7-4f2e-4fd8-bf91-2464911f2251.jpg)
![dff_const1_sim](https://user-images.githubusercontent.com/91750776/165961018-e1d71820-485b-438c-a010-ce606cff8724.jpg)
![dff_const1_netlist](https://user-images.githubusercontent.com/91750776/165961044-c6f55f22-1100-41ca-8156-8a9f733a20f8.jpg)

Example 2 :

![dff_const2_code](https://user-images.githubusercontent.com/91750776/165961121-22e92a45-c304-41a6-ab70-a30f51da08a4.jpg)
![dff_const2_sim](https://user-images.githubusercontent.com/91750776/165961146-5f7da0d7-6a62-4865-8173-66dac01292c2.jpg)
![dff_const2_netlist since q was always one no flop were required](https://user-images.githubusercontent.com/91750776/165961185-6b4eb0a5-f8d1-4aab-be39-f4a012bbfe5a.jpg)

Example 3 :

![dff_const3_code](https://user-images.githubusercontent.com/91750776/165961250-0cd9f0db-ecf5-4197-92b9-eff497d08f96.jpg)
![dff_const2_sim](https://user-images.githubusercontent.com/91750776/165961289-07fbf3e1-7f95-4181-a39b-0e8408ce6b20.jpg)
![dff_const3_netlist](https://user-images.githubusercontent.com/91750776/165961319-986fadef-9ee4-42a1-a9e5-885713babeef.jpg)

Example 4 :

![dff_const4_code](https://user-images.githubusercontent.com/91750776/165961601-15d0a157-fe8f-4237-ba4e-e8b59963e690.jpg)
![dff_const4_sim](https://user-images.githubusercontent.com/91750776/165961626-24dcfabc-0263-407f-8297-2cc0ccd66343.jpg)
![dff_const4_netlist](https://user-images.githubusercontent.com/91750776/165961651-85bf40dd-ca93-4876-aed5-030a3574aa2c.jpg)

Example 5 :

![dff_const5_code](https://user-images.githubusercontent.com/91750776/165961712-4fe380f5-8225-4335-a79e-4a21fe778b02.jpg)
![dff_const5_sim](https://user-images.githubusercontent.com/91750776/165961748-96806ae3-f098-4cfa-a4cc-3b82694aab01.jpg)
![dff_const5_netlist](https://user-images.githubusercontent.com/91750776/165961803-92080bff-9c5d-4ae1-9f39-086a0e44cf0b.jpg)

Example 6 :

![multiple_module_code](https://user-images.githubusercontent.com/91750776/165961880-82e57b8f-1fa7-476b-b102-c66563d62d90.jpg)
![multiple_modules_netlist](https://user-images.githubusercontent.com/91750776/165961912-fe3392c3-677a-4718-a20d-91fd522fafec.jpg)

Example 7 :

![image](https://user-images.githubusercontent.com/91750776/165962271-4498a1c6-cea8-4194-bd03-6583ca86c11c.png)
![counter_opt_netlist](https://user-images.githubusercontent.com/91750776/165962282-59ab12bf-5ea7-4559-9747-f20b008bd40c.jpg)


One can observe in example 2 and example 4 that none of the flip flops are invoked or used in the synthesis since, the required output is always high (1'b1) the tool has optimized as per the design. 

In case of example 7, one can observe that only the LSB [q(0)] is taken as output. Consider the Stages of a 3-bit counter as shown below :
 
 ![image](https://user-images.githubusercontent.com/91750776/165964931-8aa1474c-70c0-44e0-9f0e-d48a8dc0602b.png)

Please observe that LSB is toggling/Inverting between the states and hence the synthesis tool invoked only one FF and an inverter from the library for the netlist.
Hence optimization done for example 7.


# Part-4

 # Gate Level Simualtion(GLS) :

Gate level simulation is running the testbench against the output netlist after synthesis. Originally the RTL code was the design under test. Netlist should be logically same as the rtl code with the same set of inputs and outputs. So one can use the same testbench used for rtl simulation in gate level simulation as well.

Why GLS ?

1. Verify the logical correctness of design after synthesis.
2. To ensure the timing of the design is met.
3. For this GLS needs to be run with "delay annotation"

GLS Using Iverilog :

Refer the below diagram for approach :

![image](https://user-images.githubusercontent.com/91750776/166099183-f93c16c3-683e-4b54-8e5d-e91195545b36.png)


Synthesis Simulations Mismatches :

1. Missing signals in sensitivity list :

The sensitivity list is where you list all the signals that you want to cause the code in the process to be evaluated whenever it changes state. In sensitivity list if the appropriate parameters are not listed then the synthesis tool might develop the wrong logic.
Consider the below examples : 

Example 1 :

In this example the multiplexer is implemented using conditional operator, hence one can observe that both the Code simulation and GLS are identical.
One can distinguish between the code simulation and GLS by looking the UUT Instances in the simulator. In normal code simulation there will be only UUT under the test bench where as in case of GLS the UUT has further instances under it infered from the standard library cell. Refer the below images for more information.

CODE:

![ternary_mux_code](https://user-images.githubusercontent.com/91750776/166099878-a495aeea-c1b3-4026-9acc-ea315d29234d.jpg)

RTL Simualtion :

![ternatny_mux_xim](https://user-images.githubusercontent.com/91750776/166099893-6ecb3089-1d26-4053-b3bc-0b7738cab047.jpg)

Synthesis netlist :

![ternanry_mux_netlist](https://user-images.githubusercontent.com/91750776/166099903-95c83b8d-3f69-4018-aab3-d23e87a7f62f.jpg)

GLS :

![ternary_mux_gls_sim](https://user-images.githubusercontent.com/91750776/166099911-dd9c7a46-1241-410c-9434-b59e3b68f145.jpg)


Example 2 :

In this example one can observe that in the sensitivity list only the select line is included, the input lines are not included this will lead to wrong logic interpretation by the synthesis tool. Refer the Below images for more information.

CODE :

![bad_mux_code](https://user-images.githubusercontent.com/91750776/166100208-ccfca4e6-9b9d-4f3c-bb4d-5dbd95e7535e.jpg)

RTL Simualtion :

![bad_mux_sim](https://user-images.githubusercontent.com/91750776/166100224-4f0707ca-4f54-424e-82d9-e5416669d9ad.jpg)

Synthesis netlist :

![bad_mux_netlist](https://user-images.githubusercontent.com/91750776/166100241-9a7ad746-8b5a-4562-b36c-c3873c80e9a5.jpg)

GLS :

![bad_mux_gls](https://user-images.githubusercontent.com/91750776/166100263-e002bfcb-d6d5-4727-a88c-a365a725fc0a.jpg)

2. Blocking and Nonblocking statements :

Both blocking and nonblocking statements are procedural statements. The main difference between them is that blocking statements are evaluated in the order they are written while all non blocking statements execute concurrently (first RHS is calculated and then assigned to LHS). These difference can cause synthesis simulation mismatches due to the order in which the blocking statements are written. Consider the below example, in this case there is a delay by one clock cycle because the wire/reg X is used first and later its value is calculated and oddly blocking statments are used leading to synthesis simualtion mismatch.

CODE:

![Blocking_caveat_code](https://user-images.githubusercontent.com/91750776/166100492-9f6d1c02-0688-4ef2-958b-35a9f8d7b7a0.jpg)

RTL Simualtion :

![blocking_caveat_sim](https://user-images.githubusercontent.com/91750776/166100507-2c62eae8-deb2-4183-a6e2-307f88b6c113.jpg)

Synthesis netlist :

![blockin_netlist](https://user-images.githubusercontent.com/91750776/166100523-8764c3b7-be17-4971-b61e-87b6824d93b1.jpg)

GLS:

![blocking_caveat_glsi](https://user-images.githubusercontent.com/91750776/166100534-70310414-a852-43a6-8eda-e38c811b246f.jpg)


# Part-5

# IF, CASE, FOR LOOP & FOR GENERATE 

IF Statements: 

If statement can infer a mux or priority logic based on how its coded. Also if written badly, if statements can infer latches. Main reason for inferred latches is incomplete if statements. Let us consider the below example 1 :

Example 1 :

![incomp_if_code](https://user-images.githubusercontent.com/91750776/166153931-4f60524c-a8e9-4a11-b701-99fb37271c90.jpg)

![incomp_if_sim](https://user-images.githubusercontent.com/91750776/166153935-e6ad7d05-ec6e-471a-9a28-d3f18a50ba4e.jpg)

![synth_incomp_if](https://user-images.githubusercontent.com/91750776/166153943-16de228e-f278-432b-a025-5e917bb6d265.jpg)

![incomp_if_synth](https://user-images.githubusercontent.com/91750776/166153953-5897acee-24c3-4435-acb6-4ec3bad1fa5e.jpg)

One can observe here in the verilog code the else part of the if statement is missing, which leads to infer a latche as shown in the synthesis report and the netlist diagram. Hence it is advised to give else part mandatorily in the if statement. Let us see another example 2 as below :

Example 2 : 

![incomp_if2_code](https://user-images.githubusercontent.com/91750776/166154098-b36827b4-a9e4-4fab-8843-76d7e0ceb611.jpg)

![incomp_if2_sim](https://user-images.githubusercontent.com/91750776/166154107-abd5eb34-d11d-4f3e-8c53-233984898ad8.jpg)

![incomp_if2_netlist](https://user-images.githubusercontent.com/91750776/166154113-cecd93ef-d6cb-4202-8496-6fe2bac88648.jpg)

Case Statements :

The case statement compares an expression to a series of cases and executes the statement or statement group associated with the first matching case:

 -> case statement supports single or multiple statements.
 -> Group multiple statements using begin and end keywords.

In case of case statements if the "default" part is missed then it might be that latch can be infered, as seen in the below example : 

![incomp_case_code](https://user-images.githubusercontent.com/91750776/166154484-4207c160-004b-4e3d-83d5-d1c8a3df38aa.jpg)

![incomp_case_sim](https://user-images.githubusercontent.com/91750776/166154493-47500e0a-3b40-4547-a201-d350cf635c3d.jpg)

![incomp_case_synth](https://user-images.githubusercontent.com/91750776/166154503-07c27b0b-4cde-4ff7-af2f-f8c2a33d528c.jpg)

![incomp_case_netlist](https://user-images.githubusercontent.com/91750776/166154516-ac2ea551-e976-44fe-bf4d-7cfd008a6b46.jpg)


Consider example 3 in which default statement is used, one can easily find that latch is not being infered and appropriate result is obatined :

Example 3 :

![comp_case_code](https://user-images.githubusercontent.com/91750776/166154825-cf8fc5ee-3ae3-484d-bd9e-4b209285f08d.jpg)

![comp_case_sim](https://user-images.githubusercontent.com/91750776/166154831-de81b8f4-46a7-4070-804f-f0693cc5e65c.jpg)

![comp_case_synth](https://user-images.githubusercontent.com/91750776/166154844-57c35ebc-f60f-43f5-b6bd-2abbf5c89f60.jpg)


For loop and For generate statements :

For loop - Mainly used for evaluating expressions and can be used in always blocks.

For generate - Used for Instantiating and should be used outside always blocks. 

Consider example 4 ( demux using case statement) and example 5 (demux using for loop) below, one can easily see that both the results are same but by using for loop one can reduce the number of lines in the verilog code as well as synthesis effeciently.

Example 4 :

![demux_case_code](https://user-images.githubusercontent.com/91750776/166155093-888306a4-30a2-4425-a056-6ba882eaf1f5.jpg)

![demux_case_sim](https://user-images.githubusercontent.com/91750776/166155104-9f327f2b-9c7d-4c23-8769-bc75f66fec5a.jpg)

![demux_case_netlist](https://user-images.githubusercontent.com/91750776/166155111-7e53d4a5-6b76-464e-bc1d-c35af9e48303.jpg)


Example 5 :

![demux_generate_code](https://user-images.githubusercontent.com/91750776/166155123-dbb993f8-6ae8-4d8a-8674-29b9de175be6.jpg)

![demux_generate_sim](https://user-images.githubusercontent.com/91750776/166155131-35a1271e-90eb-4ce6-bd40-3f51c5ba9209.jpg)

![demux_generate_netlist](https://user-images.githubusercontent.com/91750776/166155138-e8fa0c96-f0a0-4682-99ea-131a88f57d00.jpg)


Atlast we have implemented 8-bit ripple carry ahead adder using For-generate statments, by instantiating 1-bit full adder 8 times as shown in the below example 6.

Example 6 :

![rca_code](https://user-images.githubusercontent.com/91750776/166155348-f00749c2-b1c3-4ba5-bcd8-cb2fb11c4bbe.jpg)

![rca_sim](https://user-images.githubusercontent.com/91750776/166155355-9b5a85b1-16e5-4136-95e4-264a2e7e4502.jpg)





# Acknowledgements

1. Kunal Gosh, Co-Founder (VSD Corp. Pvt Ltd)
2. Shon Taware, Teaching Assistant (VSD Corp. Pvt Ltd)















