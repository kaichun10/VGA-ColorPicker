___
#### FPGA Camera and VGA-ColorPicker
[![IMAGE ALT TEXT HERE](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/FPGA_VGA.PNG)](https://www.youtube.com/watch?v=ckuHAFem7OM)
___

#### Project summary list
```
A:.
|   basic_BRAM.vhd		# Defining simple BRAM for taking 8-bit RGB data and outputing 24-bit RGB data
|   color_picker.vhd		# 24 bits VGA color picker from hexadecimal value
|   controller.vhd		# FSM for output 8-bit RGB color to BRAM
|   seven_seg.vhd		# Decode input RGB 8-bit signal for 7-Segment LED display
|   pin_assignment.txt		# Pin assignment for DE1-SoC
|   README.md
|   
+---db
|
+---img
|   	Controller-BRAM.PNG
|       Top-Module.PNG       
|
+---incremental_db
|   |   README
|   \---compiled_partitions
|           
\---output_files
        color_picker.sof	# Final SRAM object file
```
___


Requirement
  - Hardware devices
    * Opal Kelly XEM6310
    + DE1-SoC
  - Software
    * Xilinx ISE 14.7 Embedded edition
    + FrontPanel API
    * Quartus-lite-20.1.1
    
___


 Set-up design environment
  - Windows 10 Xilinx ISE installation
    * [Setup local directory](#Windows-10-Xilinx-ISE-installation)
    + [Obtain and add ISE licence](#Licensing-problem-with-ISE-Editions)
  - Ubuntu 20.04.2 Xilinx ISE installation
    * [Install ISE Driver](#Install-ISE-Driver:)
    + [Using IMPACT command](#For-IMPACT-instruction:)
  - (Optional) Configure and testing in NCLaunch
    * [Compile in Cadence NCLaunch and simulating in SimVision](#Compiling-VHDL-in-Cadence-NCLaunch-and-simulating-in-SimVision)

___


#### Windows 10 Xilinx ISE installation

___
#### Licensing problem with ISE Editions

`IMPORTANT`

ISE WebPack does not support LX150 (only up to LX75) and projects do not compile in WebPack. To solve this problem, choose `ISE Embedded edition` during the installation process.

![Licence_Table_For_LX150](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/LX150_Licence_Table.JPG)


ISE Embedded edition requires a licence to activate, follow the link when adding licence for the first time and choose ISE Embedded edition on the Product licensing page.
 
![ISE_Embedded_Licence](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/LX150_ISE_Licence.PNG)


___
#### Ubuntu 20.04.2 Xilinx ISE installation
___

#### Xilinx ISE setup:

1. Install Xilinx_ISE_DS_Lin_14.7_1015_1.tar

2. Extract using 
sudo tar -xvf Xilinx_ISE_DS_Lin_14.7_1015_1.tar

3. cd Xilinx_ISE_DS_Lin_14.7_1015_1

4. sudo ./xsetup

5. Add the following line in ~/.bashrc
source /opt/Xilinx/14.7/ISE_DS/settings64.sh

6. Restart Terminal

___

#### Install ISE Driver:

Run the following command

1. sudo /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/install_script/install_drivers/./install_drivers

2. cd /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/digilent/ 

3. sudo ./install_digilent.sh

4. sudo apt-get install gitk git-gui libusb-dev build-essential libc6-dev-i386 fxload libftdi-dev

5. cd /opt/Xilinx/14.7

6. sudo git clone git://git.zerfleddert.de/usb-driver

7. cd usb-driver

8. sudo make

9. ./setup_pcusb /opt/Xilinx/14.7/ISE_DS/ISE/

___

#### For IMPACT instruction:

Run the following command

1. export LD_PRELOAD=/opt/Xilinx/14.7/usb-driver/libusb-driver.so

___

#### System Generator with Matlab:

1. cd /opt/Xilinx/14.7/ISE_DS/ISE/sysgen/util

2. sudo vim sysgen

3. Replace #!/bin/sh with #!/bin/bash

___

#### VGA output
___

#### 24 bits VGA colour picker from hexadecimal value
[![IMAGE ALT TEXT HERE](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/605e8f5edde4e-fbutube-VGA-colour-picker.PNG)](https://www.youtube.com/watch?v=D_EPlZ3a8Pw&t=2s)

___

#### Top-level schematic

![Top-level_schematic](https://raw.githubusercontent.com/kaichun10/VGA-ColorPicker/feature_add-list/img/Top-Module.PNG)
___

#### Controller-BRAM schematic

![Controller-BRAM_schematic](https://raw.githubusercontent.com/kaichun10/VGA-ColorPicker/feature_add-list/img/Controller-BRAM.PNG)
___

	Horizontal synchronous goes low between front porch and back porch
``` vhdl
    // Horizontal synchronus
	HSync : PROCESS (h_pos)
	BEGIN
		IF h_pos >= 920 THEN
			VGA_HS <= '0';
		ELSE
			VGA_HS <= '1';
		END IF;
	END PROCESS HSync;
```
___

	Vertical synchronous goes low between front porch and back porch
``` vhdl
    // Vertical synchronus
	VSync : PROCESS (v_pos)
	BEGIN
		IF v_pos >= 660 THEN
			VGA_VS <= '0';
		ELSE
			VGA_VS <= '1';
		END IF;
	END PROCESS VSync;
```

___

	Horizontal active area
``` vhdl
	HActive : PROCESS (h_pos)
	BEGIN
		IF (h_pos > 63 AND h_pos < 864) THEN
			h_active <= '1';
		ELSE
			h_active <= '0';
		END IF;
	END PROCESS HActive;
```
___

	Vertical active area
``` vhdl
	VActive : PROCESS (v_pos)
	BEGIN
		IF (v_pos > 22 AND v_pos < 623) THEN
			v_active <= '1';
		ELSE
			v_active <= '0';
		END IF;
	END PROCESS VActive;
```
___

#### ISim simulation in Xilinx ISE and testing on XEM6310 using FrontPanel Loader
[![IMAGE ALT TEXT HERE](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/605c8148bc993-fbutube-ISE.png)](https://www.youtube.com/watch?v=sqyJVjWYPRE)

___

#### Set up and configure in Quartus Lite
[![IMAGE ALT TEXT HERE](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/605c816516c03-fbutube-Quartus.png)](https://www.youtube.com/watch?v=6Hcd2ZrclaE&t=17s)

___

#### `(Opitional)`

#### Compiling VHDL in Cadence NCLaunch and simulating in SimVision
[![IMAGE ALT TEXT HERE](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/605c8581d7129-fbutube-Cadence-NCLaunch.png)](https://www.youtube.com/watch?v=sJV8FFTZp4o&t=204s)

___

#### GitLab specific runner and pipeline setup
___

#### Install and setup specific GitLab runner plus implement simple CI/CD test pipeline

[![IMAGE ALT TEXT HERE](https://raw.githubusercontent.com/kaichun10/Altera-DE1-SoC/main/img/605c846faa5f1-fbutube-GitLab-CI-CD-Pipeline.png)](https://www.youtube.com/watch?v=YLt1fnEq66U&t=225s)

___






#### Install and run GitLab runner

1. Create a folder named GitLab-Runner

2. Download .exe and rename to gitlab-runner.exe

3. Run an elevated command prompt (CMD Run as Administrator)

4. Register a runner
.\gitlab-runner.exe register

5. Install GitLab Runner
cd C:\GitLab-Runner
.\gitlab-runner.exe install
.\gitlab-runner.exe start

___

#### Run CI/CD pipeline
1. Create `.gitlab-ci.yml`

2. Write a simple test in `.yaml`

3. Git commit and push

4. Make changes and push again to test the pipeline

___
