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








