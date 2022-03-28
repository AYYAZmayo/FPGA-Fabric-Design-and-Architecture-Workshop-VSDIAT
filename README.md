# FPGA-Fabric-Design-and-Architecture-Workshop-VSDIAT
## Introduction to FPGA Architecture
## Day 1
## 4 bit Counter Implementation on Baysis 3 Board using VIVADO
The Verilog code for 4 bit counter is shown in below

![d1_counter](https://user-images.githubusercontent.com/43933912/160343477-b9ac5ec9-c57c-4cf8-90dd-b75165dcc66d.png)

First of all the simulation is performed to test the functionality of the counter using VIVADO.

![a1](https://user-images.githubusercontent.com/43933912/160255069-55ffc5e4-14cf-4aa1-a71c-1dacbb76d0c4.PNG)

The elaborated schematic of the counter is shown below
![a2](https://user-images.githubusercontent.com/43933912/160255098-732fe018-1edc-42ce-ab7f-e7c746dd1ff2.PNG)

Now the IO-planning is performed by mapping the ports of the design to baysis 3 board pins. 
clk port is mapped on W5 pin that is clock pin of 100MHz
rst port is mapped on the R2 switch
cnt[0] is mapped on U16
cnt[1] is mapped on E19
cnt[2] is mapped on U19
cnt[3] is mapped on V19

![a3](https://user-images.githubusercontent.com/43933912/160255242-b74130c4-784b-4021-a535-c88e72e3c703.PNG)

Synthesis is performed for the counter that gives the resource utilization for the counter design

![a4](https://user-images.githubusercontent.com/43933912/160255345-cd5f131b-b87a-497e-af7c-b8c406ce4ce1.PNG)

Next Implementation is performed during which the design is placed and routed on the baysis 3 board architechture. After the Implementation the final timing summary is also obtained which shows a positve slack that means the design can run on 100MHz.

![a5](https://user-images.githubusercontent.com/43933912/160255411-68c20865-6aaf-4053-ad89-764f5ce80987.PNG)

The last step is to generate the bitstream for baysis 3 board

![a6](https://user-images.githubusercontent.com/43933912/160255435-5d471721-4714-4a37-935f-a4540658eb8b.PNG)

## Virtual IO (VIO) counter on VIVADO
For obtaining the VIO in VIVDO, the "VIO IP" instance is included in the deisgn 

![a8](https://user-images.githubusercontent.com/43933912/160255483-0a193f82-9b37-4387-9e46-1605bc8db21e.PNG)

After that, again synthesis to bitstream generation flow is repeated that generates the bitstream to be loaded on the baysis 3 board.

![a9](https://user-images.githubusercontent.com/43933912/160255515-97adab74-4c66-49ad-baef-0965ab65105f.PNG)

## Day 2
## Introduction to OpenFPGA
## Running VPR flow on synthesized netlist
To run VPR flow we need to provide synthesized netlist in the form .blif and archtecture file in .xml format as shown in following command

$VTR_ROOT/vpr/vpr \
    $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
    $VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
    --route_chan_width 100

![image](https://user-images.githubusercontent.com/43933912/160256189-08941d57-ae88-400e-aeed-b4e76fad15f3.png)

VPR shows placement of blocks on gui 

![image](https://user-images.githubusercontent.com/43933912/160256536-87b1e982-4b5e-4b83-b801-e9555c725a49.png)

There are multiple options here in the GUI e.g. toggle nets  

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
Now we run the VTR flow, that is from HDL design.ODIN-II to VPR place&route and analysis for both timing and and power.
For this we have choosen a 4bit up counter in verilog.

![image](https://user-images.githubusercontent.com/43933912/160257508-43e2c2c3-c916-4bb1-a005-fbb73c009d42.png)

Run following command, it will generate several files in the directory
![image](https://user-images.githubusercontent.com/43933912/160258035-34ee6f4f-bae3-4f40-8716-ae3b5e56216e.png)

The important one is the pre_VPR.blif that is going to be used for further analysis using VPR flow

![image](https://user-images.githubusercontent.com/43933912/160258081-e1cb7066-7050-4e23-a3b4-42afe34fe9fc.png)

VPR flow running in GUI, performs the placment and routing

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

## Day 5
## RVMyth Implementation on SOFA
Use following command in generate_testbench.openfpga

![d5_2](https://user-images.githubusercontent.com/43933912/160334347-c4617c02-f76f-437e-81b0-47848d34fca0.png)

After running " make runOpenFPGA" the successful run is shown below

![d5_1](https://user-images.githubusercontent.com/43933912/160334140-c7600520-9973-48a2-8fc1-31d1a0431ff6.png)

The VPR_stdout.log will also show the status of run 

![d5_3](https://user-images.githubusercontent.com/43933912/160334618-8376c4d6-0e55-4c95-b42f-34509dd47468.png)
## Area Report of RVMyth 
VPR provides the area results 

![d5_stat_result](https://user-images.githubusercontent.com/43933912/160335243-7ee90303-36d4-4bb7-a48e-bd90c3bd230f.png)

VPR provides the detailed resource utilization of LUTs, flipflops etc. in VPR_stdout.log

![d5_utilization](https://user-images.githubusercontent.com/43933912/160335330-e8c3539c-eb2f-40e9-91fd-e1ef8f1fae24.png)

VPR also provides the detail about the logic elements in VPR_stdout.log 

![d5_Logicelements](https://user-images.githubusercontent.com/43933912/160335463-836b89ba-ecd3-463b-8c5d-ac76244b86c5.png)

## Timing analysis of RVMyth
Upon providing the sdc constraints to VPR it generates the step and hold reports for the RVMyth core. Following command is provided in the generate_testbench.openfpga for the timing analysis.

![d5_area_timing_commands](https://user-images.githubusercontent.com/43933912/160336387-2409f5d6-8efe-45d1-9733-c2202b7af5fc.png)

RVMyth core design is constrained by providing a period of 200ns. The sdc constraints are given below:

![d5_sdc_constraints](https://user-images.githubusercontent.com/43933912/160336429-517e0beb-35bb-4376-bdd2-8a0f50db1864.png)

VPR generated the Setup report 

![d5_setup_p1](https://user-images.githubusercontent.com/43933912/160336514-f6375f30-dbbe-4ed0-aa8b-34fc83c055e9.png)
![d5_setup_p2](https://user-images.githubusercontent.com/43933912/160336565-efd7db73-e336-4550-88cf-bcdef93fef4a.png)

The hold report for RVMyth generated by VPR in SOFA

![d5_hold](https://user-images.githubusercontent.com/43933912/160337050-fb42dc50-920d-478c-8bec-62ed670d00f9.png)

## RVMyth Post implementation netlist Simulation Verification on VIVADO
The post implementation netlist is generated using VPR by provinding following command:

![d5_area_timing_commands](https://user-images.githubusercontent.com/43933912/160340480-21bcc74e-373f-4db5-a695-f6cbf2500d2b.png)

The post implementation netlist

![d5_postsynthesis_netlist](https://user-images.githubusercontent.com/43933912/160340898-23481513-a6c4-4ec2-a965-92e62b5ce8d3.png)


A simple testbench is written to verify the RVMyth core funtionality. As it is supposed to give a sum of first 9 numbers at the output.

![d5_testbench](https://user-images.githubusercontent.com/43933912/160340935-30eab600-52ef-4e96-b47c-d296069843c5.png)

The simulation is performed on VIVADO by providing the primitive.v file that contains primitives used in the implementation RVMyth in SOFA. The final simulated result waveform is shown below:

![d5_rvmyth_simulation](https://user-images.githubusercontent.com/43933912/160341463-be7f962c-85ac-48b4-ac56-17d3f8efcf67.png)



