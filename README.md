# Learning-Ardupilot-Code-
Description of Ardupilot Codes

ArduPilot is an open source, unmanned vehicle Autopilot Software Suite, capable of controlling autonomous: Multirotor drones; Fixed-wing and VTOL aircraft.

<b>Basic Structure</b>

<img src = "https://github.com/sona-19/Learning-Ardupilot-Code-/blob/master/Selection_011.png" >

The basic structure of ArduPilot is broken up into 5 main parts:
<ol>
    <li>vehicle code
    <li>shared libraries
    <li>hardware abstraction layer (AP_HAL)
    <li>tools directories
    <li>external support code (i.e. mavlink, dronekit)
</ol>


<b>Vehicle Code</b>

The vehicle directories are the top level directories that define the firmware for each vehicle type. Currently there are 4 vehicle types: Plane, Copter, APMrover2 and AntennaTracker. Although There are a lot of common elements between different vehicle types, they are each different.

<b>Libraries</b>

The libraries are shared amongst the four vehicle types Copter, Plane, Rover and AntennaTracker. These libraries include sensor drivers, attitude and position estimation (aka EKF) and control code (i.e. PID controllers). 

<b>AP_HAL</b>

The AP_HAL layer (Hardware Abstraction Layer) is how we make ArduPilot portable to lots of different platforms. There is a top level AP_HAL in libraries/AP_HAL that defines the interface that the rest of the code has to specific board features, then there is a AP_HAL_XXX subdirectory for each board type, for example AP_HAL_AVR for AVR based boards, AP_HAL_PX4 for Pixhawk boards and AP_HAL_Linux for Linux based boards.



<b>Tools directories</b>

The tools directories are miscellaneous support directories. For examples, tools/autotest provides the autotest infrastructure behind the autotest.ardupilot.org site and tools/Replay provides our log replay utility.


<b>External support code<b>

On some platforms we need external support code to provide additional features or board support. Currently the external trees are:

PX4NuttX - the core NuttX RTOS used on Pixhawk boards
PX4Firmware - the base PX4 middleware and drivers used on Pixhawk boards
uavcan - the uavcan CANBUS implementation used in ArduPilot
mavlink - the mavlink protocol and code generator



<b>Ardupilot Libraries</b>

<ol>
    
    <li>Core Libraries - This includes libraries for PID control, waypoint navigation, altitude control and motors etc.
    <li>Sensor Libraries - This includes libraries for inertial sensor, barometer, optical flow, GPS, compass etc.
    <li>Other Libraries- This includes libraries for camera, buffer etc. For ex. AP_Mission library stores/retrieves mission commands from eeprom
    
 </ol>
 
 
