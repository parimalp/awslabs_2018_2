# Hardware/Software Debugging

## Introduction

This lab is continuation of the previous (**RTL Kernel Wizard**) lab. You will add ChipScope cores to monitor the acitivities taking place at the kernel interface level. 

## Objectives

After completing this lab, you will be able to:

- Add ChipScope cores at the kernel interface level 
- Debug software application
- Verify functionality in hardware on F1

## Steps
### Open an SDAccel Project
1. Execute the following commands, if not already done, in a terminal window to source the required environment settings:
   ```
      cd ~/aws-fpga
      source sdaccel_setup.sh
      source $XILINX_SDX/settings64.sh
   ```
1. Execute the following commands in a terminal window to change to a working directory:  
   ```
      cd rtl_kernel
   ```
Since we will be executing application in System configuration mode, we need to start the SDx program as being **su**
1. Execute the following commands to launch **sdx**
   ```
      sudo sh
      source /opt/xilinx/xrt/setup.sh
      /opt/Xilinx/SDx/2018.2../bin/sdx
   ```
An Eclipse launcher window will appear asking you to select a directory as workspace
1. Click on the **Browse…** button, browse to **/home/centos/aws-fpga/rtl\_kernel**, click **OK** twice

### Add ChipScope Debug core      
1. In the **Assistant** tab, expand **System > binary_container_1 > KVadd**
1. Select **KVAdd**, right-click and select **Settings...**
1. In the **Hardware Function Settings** window, click on the _ChipScope Debug_ option for the _KVAdd_ kernel
    <p align="center">
    <img src ="./images/rtlkernel_lab/FigDebugLab-1.png"/>
    </p>
    <p align = "center">
    <i>Adding ChipScope Debug module</i>
    </p>
1. Click **Apply** and **OK**
1. In the **Project Explorer** tab, expand **src > sdx_rtl_kernel > KVAdd** and double-click on the **main.c** to open it in the editor window
1. Go to line 246 and enter the following lines of code which will pause the host software execution after creating kernel but before allocating buffer
   ```
      printf("\nPress ENTER to continue after setting up ILA trigger...");
      getc(stdin);
   ```
    <p align="center">
    <img src ="./images/rtlkernel_lab/FigDebugLab-2.png"/>
    </p>
    <p align = "center">
    <i>Modifying code to stop its execution before kernel is executed to start Vivado Hardware manager</i>
    </p>
1. In the **Assistant** tab, right-click on **System** and select **Run Configuration**
1. Make sure that the Arguments tab shows **../binary_container_1.xclbin** entry
1. Make sure that the Environment tab shows **/opt/xilinx/xrt/lib** in the _LD\_LIBRARY\_PATH_ field
1. Click **Run**
The host application will start executing, loading bitstream, and pausing for the user input as coded on line 246
    <p align="center">
    <img src ="./images/rtlkernel_lab/FigDebugLab-3.png"/>
    </p>
    <p align = "center">
    <i>Paused execution</i>
    </p>
### Start Vivado Hardware Manager
1. In another terminal window, start virtual jtag connection using following two commands:
   ```
      source $XILINX_SDX/settings64.sh
      sudo fpga-start-virtual-jtag -P 10201 -S 0
   ```
The Virtual JTAG XVC Server will start listining to TCP port 10201
1. Start Vivado from another terminal window
Click on Open Hardware Manager link
Click Open Target > Open New Target
Click Next
Click Next keeping default Local Server option
Enter localhost in the Host name and 10201 in the Port fields:


