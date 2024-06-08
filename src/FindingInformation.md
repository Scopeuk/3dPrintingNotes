Notes here are focused around setting up boards with Klipper, the process for Marlin is similar but frequently requires translating from device pins to wiring pins, this is not described here.   
This page links out to a handful of manufacturer pages. These are listed as they are known to be widely used. There is no recommendation for or against anything by inclusion or exclusion from this list. If you have something you would to add raise a PR or issue.

# Klipper Repository
The Klipper repository contains a large set of example configurations, these cover configurations for a large number of printers as well as many common printer control boards.
<https://github.com/Klipper3d/klipper/tree/master/config>

# Manufacturer Github Repositories
Usually most easily found by using your search engine of choice to look for the name of your control board and the keyword github   
As an example Skr mini e3 v3 <https://www.google.com/search?q=skr+mini+e3+v3+github>   

Some manufactures are listed below for quick reference:   
Big Tree Tech   
<https://github.com/bigtreetech?tab=repositories>   

Mellow   
<https://github.com/orgs/Mellow-3D/repositories>   

MakerBase(MKS)   
<https://github.com/makerbase-mks?tab=repositories>   

Every manufacturer lays out these repositories differently. Frequently there will be a repository for each board they supply.
Inside that repository are a series of folders, typically you want to look for a pinout or pin diagram, this will list all of the pin names and what they are connected to.   
This is frequently but not always to be found in a folder called hardware.   
   
Some manufacturers will also supply basic Klipper configuration files for their boards on github.

# Manufacturer Website
Some manufacturers (notably Mellow) also maintain documentation on their own website. Sometimes this information is ahead of the [github](#manufactuer-github-repositories) particularly for very new products.
There is no standardisation around how this information is laid out.

Mellow wiki   
https://mellow.klipper.cn/#/README   

Big Tree wiki   
https://bttwiki.com/   

# Layout of Pin Diagrams
These Diagrams frequently consist of a series of tables around a picture or render of the board.
Each table will match with either a specific connector or list of a series of similar components, a single table for all the stepper drivers is common.

These tables map pin names as used by Klipper to functions of the board like stepper enable, stepper direction or thermistor input.   
Unfortunately not all of these diagrams are in a standardised format so some level of effort may have to be applied to understanding the diagram. Frequently the [Klipper Repository](#klipper-repository) will have an example configuration for a board which has all of this information converted to Klipper format. It is advised to double check any configuration file taken from the internet before use.


# Board Versions, A Word of Caution
Many manufactures issue revisions to boards throughout their lifetime, sometimes these updates can change which pins are used for certain features.
When looking up a board make sure to carefully check the silkscreen (the text printed on the board) for any version markings, this will frequently be in the format V1.2 or similar near the board name.
