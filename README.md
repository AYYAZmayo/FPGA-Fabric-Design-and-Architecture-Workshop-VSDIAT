# FPGA-Fabric-Design-and-Architecture-Workshop-VSDIAT
## Introduction to FPGA Architecture
## Day 1
## 4 bit Counter Implementation on Baysis 3 Board using VIVADO
The Verilog code for 4 bit counter is shown in below

![image](https://user-images.githubusercontent.com/43933912/160254996-7e99a01f-0e97-49e9-b8c2-041f66b1fdaf.png)

First of all the simulation is performed to test the functionality of the counter using VIVAD.

![a1](https://user-images.githubusercontent.com/43933912/160255069-55ffc5e4-14cf-4aa1-a71c-1dacbb76d0c4.PNG)

The elaborated schematic of the counter is shown below
![a2](https://user-images.githubusercontent.com/43933912/160255098-732fe018-1edc-42ce-ab7f-e7c746dd1ff2.PNG)

Now the IO-planning is performed by mapping the ports of the design to baysis 3 board pins. 
clk port is mapped on W5 pin that is clock pin of 100MHz
rst port is mapped on the R2 switch
out[0] is mapped on U16
out[1] is mapped on E19
out[2] is mapped on U19
out[3] is mapped on V19

![a3](https://user-images.githubusercontent.com/43933912/160255242-b74130c4-784b-4021-a535-c88e72e3c703.PNG)

Synthesis is performed for the counter that gives the resource utilization for the counter design

![a4](https://user-images.githubusercontent.com/43933912/160255345-cd5f131b-b87a-497e-af7c-b8c406ce4ce1.PNG)

Next is Implementation is performed during which the design is placed and routed on the baysis board architechture. After the Implementation the fimal timing summary is also obtained which shows apositve slack that means the design can run on 100MHz.

![a5](https://user-images.githubusercontent.com/43933912/160255411-68c20865-6aaf-4053-ad89-764f5ce80987.PNG)

The last step is to generate the bitstream for baysis 3 board

![a6](https://user-images.githubusercontent.com/43933912/160255435-5d471721-4714-4a37-935f-a4540658eb8b.PNG)

## Virtual IO (VIO) counter on VIVADO
For obtaining the VIO  VIVDO VIO IP instance is included in the deisgn 

![a8](https://user-images.githubusercontent.com/43933912/160255483-0a193f82-9b37-4387-9e46-1605bc8db21e.PNG)

After that again synthesis to bitstream generation is repeated that generated the bit stream to be loaded on the baysis board.

![a9](https://user-images.githubusercontent.com/43933912/160255515-97adab74-4c66-49ad-baef-0965ab65105f.PNG)

## Day 2
## Introduction to OpenFPGA
## Running VPR flow on synthesized netlist
To run VPR flow we need to provide synthesized netlist in the form .blif and archtecture file in .xml format so we ran following command

$VTR_ROOT/vpr/vpr \
    $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
    $VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
    --route_chan_width 100

![image](https://user-images.githubusercontent.com/43933912/160256189-08941d57-ae88-400e-aeed-b4e76fad15f3.png)

placemnt of blocks  on gui 

![image](https://user-images.githubusercontent.com/43933912/160256536-87b1e982-4b5e-4b83-b801-e9555c725a49.png)

There multiple options here in the GUI e.g. toggle nets 

![image](https://user-images.githubusercontent.com/43933912/160256585-80e0938a-d5d2-478c-8a04-7fdd3561e9c5.png)

Toggling the block pi utilization

![image](https://user-images.githubusercontent.com/43933912/160256613-c60eeaf2-a2f0-4dbc-80bc-b44fdf39b01e.png)

Critical path toggling

![image](https://user-images.githubusercontent.com/43933912/160256652-63ef6b51-2348-416f-a52e-cf1834ac6350.png)

After clicking on the proceed VPR flow completes.

![image](https://user-images.githubusercontent.com/43933912/160256821-ce7976d7-f409-40b6-8d82-d13e67470e24.png)

The VPR flow also performs the timing analsysis for the design. 

![image](https://user-images.githubusercontent.com/43933912/160257306-cdcf40d3-f8c0-4122-a4d5-d5f97f3a8367.png)

## Running VTR flow on HDL design of counter
Now we running the VTR flow that is from HDL design to ODIN to VPR place and route and analysis both timing and and power.
For this we have choosen a 4bit up counter in verilog.

![image](https://user-images.githubusercontent.com/43933912/160257508-43e2c2c3-c916-4bb1-a005-fbb73c009d42.png)

Run following command it wil generate several files in the directory
![image](https://user-images.githubusercontent.com/43933912/160258035-34ee6f4f-bae3-4f40-8716-ae3b5e56216e.png)

The important one is the pre_VPR.blif that going to be used for further analysis using VPR flow

![image](https://user-images.githubusercontent.com/43933912/160258081-e1cb7066-7050-4e23-a3b4-42afe34fe9fc.png)

VPR flow running in GUI performs the placment and routing
![image](https://user-images.githubusercontent.com/43933912/160258359-5c9f5ec0-410f-4c27-a37b-7631016e9b22.png)

Post sythesis simulation is performed on the counter using VIVADO. Following command is used to generate postsyhtnesis netlist

![image](https://user-images.githubusercontent.com/43933912/160258633-5e854210-b238-4257-b249-703c29b83017.png)

Timing Analysis

![image](https://user-images.githubusercontent.com/43933912/160258922-8ddb3ed5-f6f0-43de-94c6-ea24525a2a72.png)


Timing anlaysis is performed by providing the SDC contraints

## Day 3
## RVMyth Core RTL to Synthesis on VIVADO
•	This core has several modules the main module is Core which has 3 inputs called clk, reset and output that is of 8 bits.
•	This adds the first 9 numbers and gives an output of 45 
•	The code is given here in RISCV Instructions based assembly code

![f1](https://user-images.githubusercontent.com/43933912/160259221-8f014e07-78aa-4382-abf1-1dcfd6c47da0.PNG)

•	These instructions are saved in the instruction memory as binary format as shown in below figure. It is a five stage pipelined processor in which the fetch unit fetches these instructions one by one then the decode unit decodes them and the execution unit or ALU performs the execution of these instructions and access the memory and at the end write back is performed.

![f2](https://user-images.githubusercontent.com/43933912/160259336-c09679df-7c29-4f3a-b0a3-7e5df65ee382.PNG)
 
•	Create a simple test-bench for testing the core present in the Code folder as test.v

![f3](https://user-images.githubusercontent.com/43933912/160259349-99664dca-6b04-4690-8991-aaee375cdefe.PNG)

•	Now simulate the Core in VIVADO by creating a new project and selecting baysis 3 as the Board. Add the main file myth_test.v  in design sources section and testbench file in sources section.

![f4](https://user-images.githubusercontent.com/43933912/160259368-bb2e89c6-1b5f-4dd6-a882-0030c3145574.PNG)

•	Now perform Behavioral simulation for the design as shown in the below waveform.  It shows the required output as we are expecting the sum of first 9 numbers as 45.

![f5](https://user-images.githubusercontent.com/43933912/160259384-f9d7126c-6dc0-4ec6-baa4-a7847d802955.PNG)

•	Now elaborating the design. Once the elaboration is done open the IO planning and assign the W5 clock of 100MHz of Baysis 3 Board to "clk" of the core and also assign the "reset" to a switch R2. And also set the IO standar as LVCMOS33. Here I don’t  assign “out”  to LEDs rather I observe it on ILA.

![f6](https://user-images.githubusercontent.com/43933912/160259450-eee88a70-6989-445c-8676-aa788588daa9.PNG)

•	For observing the output on ILA I have to remove “out” port from the module definition and perform the elaboration again and now in the IO planning only the clk and reset will appear.
•	Before performing the synthesis we also add the timing constraints of 100 MHz using the constraint wizard in synthesis column. Below is the utilization summary of the synthesized design

![f7](https://user-images.githubusercontent.com/43933912/160259469-4060a7cd-1339-41fe-818c-4c3bf0fe22c9.PNG)

•	Now perform Implementation and final implemented design after place and route

![image](https://user-images.githubusercontent.com/43933912/160259498-532e5968-6e17-450c-97fe-1960b1b6c120.png)

•	The final timing summary of the MYTH core is obtained as

![image](https://user-images.githubusercontent.com/43933912/160259506-93d2103c-7de9-465e-bce6-d7738461d255.png)

## Day 4
## Introduction to SOFA FPGA Fabric IP


![l4_1](https://user-images.githubusercontent.com/43933912/160259586-e445abed-672b-4e40-bf3d-08ab8faf7450.PNG)


![l4_2](https://user-images.githubusercontent.com/43933912/160259609-c834a48b-aa38-4533-83c6-2cdb1fad9820.PNG)


![l4_3](https://user-images.githubusercontent.com/43933912/160259620-be522ee0-6fde-4b19-9c6a-738ca183416d.PNG)


![l4_4](https://user-images.githubusercontent.com/43933912/160259625-f2107683-e3e9-416d-92b3-22cd5fa09f0e.PNG)


![l4_5](https://user-images.githubusercontent.com/43933912/160259629-2575c63a-20b9-4249-82d5-6eb1f05922ce.PNG)


![l4_6_utilization](https://user-images.githubusercontent.com/43933912/160259641-e58d045e-2f26-4645-9e98-15b693136e83.PNG)


![l4_7](https://user-images.githubusercontent.com/43933912/160259647-74edf9ba-20de-424f-86a2-01855295fc32.PNG)


![l4_8](https://user-images.githubusercontent.com/43933912/160259655-25ea3080-cfda-4294-ab22-ef8780c0b4da.PNG)


![l4_9](https://user-images.githubusercontent.com/43933912/160259666-fc0afb2e-3ed3-4068-81bb-ba13d746a533.PNG)


![l4_10_timing](https://user-images.githubusercontent.com/43933912/160259670-8b004ba8-aa47-449a-9ea1-d4615964d223.PNG)


![l4_11_hold](https://user-images.githubusercontent.com/43933912/160259679-e15eaaaf-7fa1-4994-bc13-679a0b28b047.PNG)


![l4_12](https://user-images.githubusercontent.com/43933912/160259687-235c0990-7d0f-4f1c-8df0-34295d44d537.PNG)


![l4_13](https://user-images.githubusercontent.com/43933912/160259690-0a78c0b2-ec00-40fe-be93-c902895cd23a.PNG)


![l4_14_post_simulation](https://user-images.githubusercontent.com/43933912/160259771-b6c9a506-c97b-4df9-aac2-b71d28b1031b.PNG)








