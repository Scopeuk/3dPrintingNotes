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

Some older boards did not provide a method for connecting to the diag pin and this had to be done with jumper wires, modern controller boards have moved away from this.


## Software
Klipper has special sections in it's configuration for configuring TMC drivers see <https://www.klipper3d.org/Config_Reference.html#tmc-stepper-driver-configuration>
The Klipper guide to setting this up is at <https://www.klipper3d.org/TMC_Drivers.html#sensorless-homing>

The bits of interest to us are the threshold and the diag pin name.   
The diag pin must be set to match the layout of the board.    
The threshold must be tuned per machine, there are many factors which affect this. The name of the threshold variable varies by driver, see Klipper docs.

once the TMC section has been added a new "virtual pin" becomes available within Klipper the name of this is given in the Klipper section but will be similar to "tmc2130_stepper_x:virtual_endstop" where the TMC model number and stepper name with change with the associated section in the configuration.
This new virtual pin can be used as the end stop pin in the normal stepper section for the axis.
