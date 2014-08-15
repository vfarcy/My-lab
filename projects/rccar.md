###### August 15th, 2014

Building a programmable RC Car
------------------------

![](http://upload.wikimedia.org/wikipedia/en/thumb/1/1c/Car_collection.jpg/300px-Car_collection.jpg "Not yet done !")

The purpose of the project is to modify the remote control of a cheap RC car so it can be controlled by software. 

An arduino will be wired to the remote control. It will pilot the remote controller using a very simple language :  

- Directions : FF = Forward, FR = Forward & Right, BB = Backward, etc.
- Stop : SS = Stop
- Duration in milliseconds
- Speed : ranges from 0 to 255

For example a very simple program to drive forward at full speed (255) for 100 ms will look like that : `FF, 255, 100`. Complex motions will be possible by sending longer programs. As a cherry on the cake, the remote controller will be accessible via a browser so it can be possible to (blindly !) monitor the car through an internet connection (a next step could be to  stream a video captured by a camera).





