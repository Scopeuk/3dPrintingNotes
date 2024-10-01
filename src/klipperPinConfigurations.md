This section aims to provide additional clarity or expansion of <https://www.klipper3d.org/Config_Reference.html#format-of-micro-controller-pin-names> this does not replace the klipper documentation but augments it and expands upon reasoning.
# Pin Names
Pinnames are specific to the controler in use, typically they are in one of a handful of formats  and depend on the type of microcontroler used on the control board. The format is not itself important but must match the defintions used for your control board.   
A few common formats are listed below:    
PB1 - Two letters followed by a number in this case Port B Pin 1. This is common on STM32 and AVR based controlers.   
P1.1 - Port 1 Pin 1. This is common on LPC based controlers.   
gpio1 - gpio followed by a number. This is common on RP2040 based controlers and raspberry pi mcu.   

If any of this is confusing or intimindating, ignore it. The steps needed to get this working are setting the name to match what the manufacuter documentation states, what it all means is unimportant.
Some help on where to look for this information is avaliable in [Finding Information](FindingInformation.md)

# Modifiers
Modifiers are added to a pin name in a configuration to change how the pin behaves

| Modifier | Effect | 
| ------------ | ------------ |
| ! | Invert the pin | 
| ^ | Enable Pull up Resistor |
| ~ | Enable Pull down Resistor |

## Inverting Pins
Many devices on a 3d printer controler use boolean, true and false signals which map to on or off on a pin on the micro controler,   
The chips which recived this signal  (for instance stepper drivers) assign specific meaning to on and to off. Inverting the pin lets us map klippers idea of what is on to the same thing the chip expects.
Frequently this is used with the direction pin on a stepper driver, if on makes the motor rotate clockwise, off will make it rotate counter clockwise.
   
Normal: pin1

| software | pin | effect |
| ------------ | ------------ | ------------ |
| true | on | clockwise |
| false | off | counterclockwise |

Inverted: !pin1

| software | pin | effect |
| ------------ | ------------ | ------------ |
| true | off | counterclockwise |
| false | on | clockwise |

This changes the signal recieved by external hardware but not how the software see's the world. This is particularly useful for devices which use "active low" signals, this is very common and simply means that the pin being "off" will make the device run. Stepper driver enable signals are frequently active low
This can also be used on input pins, this is commonly done for "normally open" endstops to make them false when not pressed and true when pressed.


## Why use pull resistors?
This section talks about a pull up resistor as this is by far the most common.      
Think about a common endstop circuit, this has a switch which connects ground to a pin of the micro controler on the printer control board.   

![Circuit Diagram No Pull Up Resistory!](Images/WithoutPullUp.svg "Circuit without pullup")   

This switch can make a circuit connecting the pin to ground or it can break the circuit.
This might seam to give us both on and off, but what we actually have is off (pin connected to ground) and unknown, when there is no circuit there is nothing to make the pin on.   
The pull up resistor is part of the micro controler on the printer control board which connects the pin to a resistor connected to the on voltage.    

![Circuit Diagram With Pull Up Resistory!](Images/WithPullUp.svg "Circuit with pullup")    

This resistor will mean that when we break the circuit the pin goes to on, but as this is a faily high resistance if we add a low resistance connection to ground (press the switch) the pin will go to off. This gives us our off (switch and circuit connected) and on (switch and circuit disconnected, resistor pulls to on instead).

## Pull Up Resistors
When the pin is not connected this will make the pin go to on.
All klipper controlers support pull up resistors

## Pull Down Resistors
When the pin is not connected this will make the pin go to off.
Support for pull down resistors is limited. It is much more common to use pull up.

## Combing Modifiers
The ! modifier can be combined with either ^ or ~ on an input pin (one taking information from the world like an end stop switch).
adding both ^! or ~! will cause both effects to happen.
When combining operators the order is pull (up or down, ^ or ~) and then the inversion operator !.   

| Modifier | Effect | 
| ------------ | ------------ |
| ^! | inverted and pull up enabled |
| ~! | inverted and pull down enabled |