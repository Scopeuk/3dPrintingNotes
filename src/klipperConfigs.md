# Getting Started With printer.cfg
Klipper configuration files can appear to be intimidating at a first attempt. The files however are laid out in sections with each section self-contained. As such the sections can be dealt with one at a time and it is not necessary to understand everything to get started. This is not aimed at walking you through creating a configuration file from scratch but acts a guide to how to read an existing file and file the details of what each section does.
## Before We Start
Changes to your printer.cfg file can stop your printer working, before making any changes take a backup copy of the file. In the worst case you can always swap back to the backup.

## What is printer.cfg
printer.cfg is the configuration file Klipper reads when it first starts. This configuration file tells Klipper how your printer is configured. This is how Klipper can tell apart an Ender 3 and a Voron 2.4 (although Klipper really doesn’t care).
It is responsible for documenting the physical configuration of the machine, for instance it's dimensions and kinematics, electrical setup like the type of stepper drivers used and how they are wired up on the controls board, and finally any Klipper settings like [arc support](https://www.Klipper3d.org/Config_Reference.html?h=arc#gcode_arcs)  or the [virtual sd card](https://www.Klipper3d.org/Config_Reference.html?h=virtu#virtual_sdcard).

## CASE SeNsItIvItY
All Klipper configuration files are case sensitive. What this means is that all of the following names are different.   
test   
Test   
TEST   
TeSt   
This can be a little tricky as in normal English these are all the same word. The whats and whys of this are unimportant, just make sure it matches and you'll be fine.

## Comments
Klipper configuration files allow for "comments" these are bits of text put in for the benefit of the humans reading the file.   
Comment lines start with \# everything after \# is ignored by Klipper.   
There is one special case exception to this [SAVE_CONFIG](#save_config).    
This is used extensivly within the [Klipper configuration reference](https://www.Klipper3d.org/Config_Reference.html) where the details of each parameter are documented in comments, these can be included in your printer configuration along with any of your own notes to help you understand.

## File Structure
The general structure of a printer.cfg file will be
some lines starting \[include. see [Include Lines](#include-lines)   
followed by a series of series of sections which start with \[section name\]. see [Configuration Sections](#configuration-sections-lines-starting-word-or-word-anotherword)   
A line in the form    
\#\*# <---------------------- SAVE_CONFIG ---------------------->   
A series of lines startin with #*# . see [SAVE_CONFIG](#save_config)   

### Include Lines
A line starting \[include is being used to "include" another configuration file. Effectively what this does is add the included file at this point in the current file.   
An example include   
[include anExtraFile.cfg]   
   
So what does this do?   
This is equivalent to copying the whole contents of anExtraFile.cfg into our printer.cfg starting at this line. 
     
Why would you do this?   
This may seem like just complexity for the sake of it however this is a powerful way to allow us to separate out infrequently used functionality or functionally which is grouped together.
Everyone uses this differently, just know that if there is an \[include fileName.cfg\] line that there is another configuration file to look at.

### Configuration Sections (lines starting \[word\] or \[word anotherWord\])
Klipper functionality is split into a series of sections to configure each element of your printer. I will not talk about each section here as they are documented in the [Klipper configuration reference](https://www.Klipper3d.org/Config_Reference.html).   
This always be your starting point for understanding what the section does. Some additional notes about Klipper pin configurations are available in [Klipper Pin Configurations](KlipperPinConfigurations.md)   
A typical configuration file will have 10's or possibly hundreds of these sections depending on the complexity of the printer and the features used.


Each section starts with the section name inside a square brackets for example called myHotend would be on a new line starting \[myHotend\]
This heading can optionally have an "instance name", this allows things like fans to have a unique name that can be used elsewhere in Klipper. An example is given below   
\[heater_fan heatbreak_cooling_fan\]   
This represents a section "heater_fan" which can be looked up in the Klipper configuration reference which has been named "heatbreak_cooling_fan".   
This heading will then be followed by a series of lines setting named parameters to values.   
These lines are written in the format "name: value" where name is the name of the parameter and value is the value to assign to that parameter
The list of parameters for each configuration section is specific to that section and must be looked up in the [Klipper configuration reference](https://www.Klipper3d.org/Config_Reference.html)

#### Example section
Below is an example configuration section, specifically this is the printer section for an ender 3:
\[printer\]    
kinematics: cartesian   
max_velocity: 300   
max_accel: 3000   
max_z_velocity: 5   
max_z_accel: 100   

Let’s break this down:   
The section is called printer see [printer in the Klipper configuration reference](https://www.Klipper3d.org/Config_Reference.html?h=printer#printer)   
This section has five parameters:   
kinematics which is set to cartesian
max_velocity which is set to 300   
max_accel which is set to 3000   
max_z_velocity which is set to 5
max_z_accel which is set to 100

There is nothing to mark the end of a section, this is done implicitly at the end of the file or when finding the start of the next section.

What each of these values means or does is left to the Klipper configuration reference as anything recorded here will inevitably get out of date and the purpose of this is to help you find information and get started with understanding your printer configuration.   

# SAVE_CONFIG
Klipper will sometimes save some data for us, things like your Z offset or PID tuning parameters.
In order to do this Klipper has to put the data somewhere and the somewhere is the end of printer.cfg.
Klipper helpfully puts a line in the configuration file to tell us that it has done this and that lines is    
\#\*# <---------------------- SAVE_CONFIG ---------------------->   
Every line after this will start with "#\*# " but you will notice that the things after "#\*# " look exactly the same as the normal configuration settings.   
That is because they are, Klipper will read the configuration (lines without "#\*# ") and then read the lines starting with "#\*# " to see if anything has been updated with a saved setting.
Normally you should not need to edit after the SAVE_CONFIG line but under some error conditions (normally relating to a change of hardware) where you remove a user created section it may be necessary to find and delete the matching saved settings section.


# Terrifyingly complex looking stuff full of {}'s and %'s
This is almost certainly some sort of G Code macro, these can be quite complex and are fundamentally a programming language. Most users will simply use macros created for a specific job by someone else.
You can confirm you are looking at one of these section by scrolling up from the section until you find the section heading, it should start \[gcode_macro.
These are not documented here the Klipper documentation starts at <https://www.Klipper3d.org/Config_Reference.html?h=macro#g-code-macros-and-events> and the details of all the content can be found at <https://www.Klipper3d.org/Command_Templates.html>.
This is an advanced topic and should not be needed to get a printer started outside of pre-defined macros supplied by the printer designer or manufacturer.

