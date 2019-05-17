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


<b>External support code</b>

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
 
 

<b>Library Example Sketches</b>

The first step in exploring the code for yourself is to use the example sketches for the libraries. 
Knowing the library API and conventions used in ArduPilot is essential to understanding the code. So using the library example sketches is a great way to get started. As a start you should read, build and run the example sketches for the following libraries:

    libraries/AP_GPS/examples/GPS_AUTO_test
    libraries/AP_InertialSensor/examples/INS_generic
    libraries/AP_Compass/examples/AP_Compass_test
    libraries/AP_Baro/examples/BARO_generic
    libraries/AP_AHRS/examples/AHRS_Test
    
    
 <b>Understanding the example sketch code</b>

When you are reading the example sketch code (such as the GPS_AUTO_test code) you will notice a few things that may seem strange at first:

    it declares a ‘hal’ variable as a reference
    the code is quite rough and not well commented
    the setup() and loop() functions

The hal reference

Every file that uses features of the AP_HAL needs to declare a hal reference. That gives access to a AP_HAL::HAL object, which provides access to all hardware specific functions, including things like printing messages to the console, sleeping and talking to I2C and SPI buses.

The actual hal variable is buried inside the board specific AP_HAL_XXX libraries. The reference in each file just provides a convenient way to get at the hal.

The most commonly used hal functions are:

    hal.console->printf() to print strings
    AP_HAL::millis() and AP_HAL::micros() to get the time since boot
    hal.scheduler->delay() and hal.scheduler->delay_microseconds() to sleep for a short time
    hal.gpio->pinMode(), hal.gpio->read() and hal.gpio->write() for accessing GPIO pins
    I2C access via hal.i2c
    SPI access via hal.spi

Go and have a look in the libraries/AP_HAL directory now for the full list of functions available on the HAL.
The setup() and loop() functions

You will notice that every sketch has a setup() function and loop() function. The setup() function is called when the board boots. The actual call comes from within the HAL for each board, so the main() function is buried inside the HAL, which then calls setup() after board specific startup is complete.

The setup() function is only called once, and typically initialises the libraries, and maybe prints a “hello” banner to show it is starting.

After setup() is finished the loop() function is continuously called (by the main code in the AP_HAL). The main work of the sketch is typically in the loop() function.

Note that this setup()/loop() arrangement is only the tip of the iceberg for more complex boards. It may make it seem that ArduPilot is single threaded, but in fact there is a lot more going on underneath, and on boards that have threading (such as Pixhawk and Linux based boards) there will in fact be lots of realtime threads started. See the section on understanding ArduPilot threading below.
The AP_HAL_MAIN() macro

You will notice a extra line like this at the bottom of every sketch:

AP_HAL_MAIN();

That is a HAL macro that produces the necessary code to declare a C++ main function, along with any board level initialization code for the HAL. You rarely have to worry about how it works, but if you are curious you can look for the #define in the AP_HAL_XXX directories in each HAL. It is usually in AP_HAL_XXX_Main.h.



To understand library sketch, one can look at [AP_GPS_AUTO](https://github.com/ArduPilot/ardupilot/edit/master/libraries/AP_GPS/examples/GPS_AUTO_test/GPS_AUTO_test.cpp)





<b>SENSOR DRIVERS</b>

How sensor drivers are written and integrated into the vehicle code.

I2C, SPI, UART (aka Serial) and CANBUS (in particular UAVCAN) protocols are supported. If you plan to write a new driver, you will likely need to refer to the sensor’s datasheet in order to determine which protocol it uses.
