# Leapfrog-Create-Upgrade-with-BigTreeTech-Parts
This is a tutorial on upgrading the Leapfrog Create 3D printer with Big tree tech octopus pro motherboard, TMC5160 and 2209 drivers, Big tree tech TFT 50 display and Marlin 2.1 firmware

# ///Useful links///
https://github.com/MarlinFirmware/Marlin - Firmware

https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro - Motherboard 

https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware - Display 

https://github.com/bigtreetech/BIGTREETECH-Stepper-Motor-Driver/blob/master/TMC2209/V1.2/manual/TMC2209-V1.2-manual.pdf - TMC2209 manual

https://github.com/bigtreetech/BIGTREETECH-Stepper-Motor-Driver/blob/master/TMC5160/manual/BIGTREETECH%20TMC5160-V1.0%20manual.pdf - TMC 5160 manual

# ///Components used///
1x BigTreeTech Octopus Pro Motherboard
1x BigTreeTech TFT50 Display
5x XH2.54 4Pin to 6Pin Stepper Motor cables
1x XH2.54 2pin Fan cable
3x TMC 2209 Stepper Motor Drivers
2x TMC 5160 Stepper Motor Drivers
3x 2 pin 2.54 End stop cable
8GA wire

# ///Firmware///
You will need Microsoft Visual Studio Code and the PlatformIO and Marlin AUTO builder extensions.
Download the firmware folder from the link provided above along with the two config files provided in this repository. You will have to replace the already existing config files in the Marlin folder with the ones you downloaded. In the Marlin bugfix0-2.1.X firmware folder you downloaded, navigate to the Marlin subfolder and paste **Configuration.h** and **Configuration_adv.h** files in there by replacing the existing ones. You **WILL WANT TO** configure the E-STEPS, PID and the z and y offsets for your particular machine as they are not preconfigured. They can be configured from the printer after flashing the firmware or from the configuration files.
Click on the M icon on the right and you will get a screen like this one 
![image](https://github.com/HD1D1/Leapfrog-Create-Upgrade-with-BigTreeTech-Parts/assets/124192839/d9d1e11e-06d4-4fae-982b-be8d309a19dc)
Check your motherboard to see what chip do you have and select the appropriate environment. After that, you can flash the .bin file with an sd card by inserting it in the sd card slot on the motherboard. The .bin file should be the only thing on the cd card. After a successful flash, the file will be renamed .CUR

# ///Hardware///
### Drivers
We will start with the driver configuration. The max current that the Leapfrog consumes is 1.8 Amps, which the TMC 2209 is capable of handling, but to be on the safe side we have selected the more powerfull TMC5160 drivers for the X and Y axis.
Firstly the motherboard can provide up to 60V to the drivers, which **none** of the drives are capable of handling so you need to make sure that  the jumpers that control the voltage are in the right position. They are located at the top of the driver sockets.
![stepper_24V](https://github.com/HD1D1/Leapfrog-Create-Upgrade-with-BigTreeTech-Parts/assets/124192839/eb510c7d-1979-41b7-8eed-52253bb1ef94)
The Driver sockets are configured as the following: MOTOR -> X , MOTOR1 -> Y , MOTOR2_1 -> Z , MOTOR3 -> E0 , MOTOR4 ->E1
You will use the 5160 for X and Y and the rest are TMC 2209. For all future reference to pins and sockets consult the diagram below.
![PRO-__04](https://github.com/HD1D1/Leapfrog-Create-Upgrade-with-BigTreeTech-Parts/assets/124192839/67b7007e-044b-455b-86df-35ca3ebfc837)
Before putting the drivers themselves you will need to configure their modes. TMC2209 use UART and TMC5160 use SPI.
![PROmotor mode](https://github.com/HD1D1/Leapfrog-Create-Upgrade-with-BigTreeTech-Parts/assets/124192839/2d0f8914-9c90-4a1b-b17f-e4552708daf4)
The Y axis is controlled by two stepper motors which are driven by a single driver. As such you will need to mirror the connection, so the motor spins backward, by cutting the 4-pin connector of one of the cable and soldering red to black, black to red, blue to green, and green to blue as shown below
![scematic](https://github.com/HD1D1/Leapfrog-Create-Upgrade-with-BigTreeTech-Parts/assets/124192839/f18c6e9d-4327-476b-b9e6-09a018df19b6)

#### Endstops
You will need to cut the old connectors of the end stops as they don't fit on the new motherboard and solder the 2 pin 2.54 female ends to them. I have decided not to use the factory Z endstop sensor, and instead made a switch and placed it on a platform at the front left corner of the bed, but you can use the factory one if you wish.
Find the edstop pins on the wiring diagram above. ENDSTOP0 is for X , 1 for Y and 2 for Z. You have to plug the connectors in the bottom two pins of the ports.

### Fans
The connectors ones again don't fit the ports on the motherboard. cut the end and solder the XH2.54 2pin connectors. Above the fan ports are jumpers to select the voltage. The fans on the leapfrog are 24V
![PRO-fan pins](https://github.com/HD1D1/Leapfrog-Create-Upgrade-with-BigTreeTech-Parts/assets/124192839/a19ab636-073d-4028-8dbc-7a47a9f94ef9)

### Thermistors
The thermistor connectors are compatible so no need to change anything. Find the ports from the wiring diagram. TB is for the bed , T0 for the first extruder, T1 for the secound. 

### Display
We will use the TFT connection of the display. Plug the black cable that comes in the box in the RS232 on the back of the display ![bigtreetech-tft50-v30-dual-mode-display-5-inch](https://github.com/HD1D1/Leapfrog-Create-Upgrade-with-BigTreeTech-Parts/assets/124192839/1107c64c-5250-4027-bacf-806dc001c891)
and and the other end in the TFT port on the motherboard. One of the cables is apart from the others and should be connected to the bottommost pin of the port.
There are two common problems with the display being unable to communicate with the motherboard. One is that the baud rate is incorrect. The baud rate is set from the firmware config file. In our case, I have set it **250000**. It should be the same in the display settings, otherwise, the devices cant communicate. You can change it in the Settings > Connection tab on the display. The other problem may be the "TFT is in listening mode" massage you may get. The problem is that the display is not connected. You can connect it from the same tab.

### Power
**Unplug** the power supply befor doing anything with it! Connect the BED_OUT terminals on the motherboard with the cables that hang from the bed. The polarity of the terminals should be displayed  either on top of the plastic protector or on the bottom of the motherboard. Red should be + and black -. Connect the rest of the terminals to the power supply. V- to - and V+ to +.
