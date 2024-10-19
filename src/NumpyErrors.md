# Numpy Errors
## Background
Numpy is a maths library used as part of python the programming language Klipper is written in, well the bits that run on the host anyway.
This is used during calculations as part of input shaper calibration.   
   
There are two basic errors that come up, they have very similar display error messages.   
   
## Symptoms
Klipper log displays "Failed to import \`numpy\` module".   
This will be followed by a message stating make sure it is installed via pip install this message will also include a path to pip something like ```'~/klippy-env/bin/pip'```
There are two root causes of this:    
1). numpy is not installed.   
2). A library upon which numpy depends (lib blas) is not installed.
   
## So how do we fix this? 
It is possible to dig through Klipper logs and issue commands to determine which issue is at play, but it is as quick and easy to try and fix issue 1 first, the solution for issue 1 will highlight if issue 2 is at play.
It is not uncommon to need to fix both of these issues depending upon how you installed your initial Klipper image.   
     
### Issue 1
To fix issue 1 we install numpy this should be done using the pip path given in the Klipper error message.   
The command to install will look like the command below with the path modified to match your Klipper error message.
```~/klippy-env/bin/pip install numpy```   
This command can take a long time to run as various things need to be built. If this command returns a message which states "Requirements already satisfied" numpy is already installed and you actually have issue 2.

### Issue 2
To fix issue 2 we need to install the supporting library libblas. This is done through the apt command, command below. If you dont understand what this is doing see [Linux For Klipper Users](linuxForKlipperUsers.md)     
```sudo apt install libopenblas0```   
Note: the name of the package libopenblas0 has changed relatively recently the package used to be called libopenblas-base. if you tried installing libopenblas-base and the command failed this is why.

