###### August 15th, 2014

Building a programmable RC Car
------------------------

![](http://upload.wikimedia.org/wikipedia/en/thumb/1/1c/Car_collection.jpg/300px-Car_collection.jpg "Not yet done !")

The purpose of the project is to modify the remote control of a cheap RC so it can be controlled by software. 

An arduino will be wired to the remote control and will pilot it using a very simple language :  

Directions : FF = Forward, FR = Forward & Right, BB = Backward, etc.

Stop : SS = Stop

Duration in milliseconds

Speed : ranges from 0 to 255

A single line instruction is as follows : <Direction>, <Speed: Range 0 - 255>, <Duration in Milliseconds>

For example to drive forward at full speed (255) for 100 ms, the one line program will looks like `FF, 255, 100`.

More complex programs can be made possible :

```# Drives forward at full speed (255) for 100 ms
#
FL, 255, 250
FR, 255, 400
FL, 255, 100```


