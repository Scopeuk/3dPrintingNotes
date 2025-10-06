# What is Sensorless Homing
Sensorless homing uses the "stall guard" feature of the Trinamic TMC family of drivers to detect when the stepper motors are unable to turn.   
In 3d printing we use this to detect when an axis or carriage has reached the end of its travel.   
The TMC drives have a single Diagnostic pin known as "diag" which is used to communicate status of the driver. In typical 3d printing configurations only the stall guard system is using this pin.   

The process is typically as follows (although in the real world it happens very quickly)

1. The tool head hits the frame (or some stopper attached to the frame) and stops.
2. The tool head stops the belt connected to the tool head.
3. The belt stops the motor driving the tool head.
4. The motor driver detects that driving the motor suddenly became harder.
5. The motor driver sets its diag pin true.
6. Dependant on controller board, the diag signal crosses the diag jumper
7. The controller sees the diag pin go true and considers this like an end stop.


# How is it setup
## Electrically
There are two main ways of connecting the diag pin to the MCU, 
1. The board provides dedicated pins on the MCU permanently connected to the motor driver diag pins.
2. The board provides jumpers which allow the diag pins to be optionally connected to the MCU, typically in this configuration the jumpers will connect the diag pin to the associated end stop input.

## External Drivers / Step Sticks
Some external drivers, have a jumper or switch to connect the diag pin of the tmc driver to the step stick socket the driver is in.
In order to use sensorless homing this jumper must be installed or the switch must be switched on.

Some older boards did not provide a method for connecting to the diag pin and this had to be done with jumper wires, modern controller boards have moved away from this.


## Software
Klipper has special sections in it's configuration for configuring TMC drivers see <https://www.klipper3d.org/Config_Reference.html#tmc-stepper-driver-configuration>
The Klipper guide to setting this up is at <https://www.klipper3d.org/TMC_Drivers.html#sensorless-homing>

The bits of interest to us are the threshold and the diag pin name.   
The diag pin must be set to match the layout of the board.    
The threshold must be tuned per machine, there are many factors which affect this. The name of the threshold variable varies by driver, see Klipper docs.

Once the TMC section has been added a new "virtual pin" becomes available within Klipper the name of this is given in the Klipper section but will be similar to "tmc2130_stepper_x:virtual_endstop" where the TMC model number and stepper name with change with the associated section in the configuration.
This new virtual pin can be used as the end stop pin in the normal stepper section for the axis.


# Common Issues
The following are a list of issues which have been observed setting up sensorless homing which make it "not work" in decending order of how often they have come up.
1. Missing diag jumpers   
		see [How is it setup, electrically](#electrically)   
2. Missing jumper/switch on external drivers   
		see [External Drivers / Step Sticks](#external-drivers--step-sticks)
3. Incorrect diag pin set in tmc driver configuration section   
		see [Software](#software)
4. Endstop pin for stepper not set to correct tmc driver virtual endstop   
		see [Software](#software)
5. Sensitivity too high.   
		This is rare but if the sensitivity is high enough between klipper and the driver the pulse to stop the driver can be missed causing the homing to run until crash, it is suspected that this is caused by the pulse happening too early for klipper to catch but is not confirmed at this time.   
		If your starting tuning and having issues getting the device to stop it is worth trying a low sensitivity as well as high ones.