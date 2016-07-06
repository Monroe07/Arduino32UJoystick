### BETA Testing: This branch is currently being tested. All functions have been implemented and optimized. Feel free to try it out and let me know if you have any issues. The examples are not 100% up-to-date yet.

# Arduino Joystick Library
#### Version 2.0.0 (in Beta Testing)
This library can be used with Arduino IDE 1.6.6 (or above) to add one or more joysticks (or gamepads) to the list of HID devices an [Arduino Leonardo](https://www.arduino.cc/en/Main/ArduinoBoardLeonardo) or [Arduino Micro](https://www.arduino.cc/en/Main/ArduinoBoardMicro) (or any Arduino clone that is based on the ATmega32u4) can support. This will not work with Arduino IDE 1.6.5 (or below) or with non-32u4 based Arduino devices (e.g. Arduino UNO, Arduino MEGA, etc.).

##Installation Instructions
Copy the Joystick folder to the Arduino libraries folder (typically `%userprofile%\Documents\Arduino\libraries`). On Microsoft Windows machines, the `deploy.bat` file can be executed to install the Joystick folder (assuming a default Arduino installation). The library should now appear in the Arduino IDE list of libraries. The examples should also appear in the examples menu in the Arduino IDE.

##Examples
The following example Arduino sketch files are included in this library:

- `JoystickTest` - Simple test of the Joystick library. Exercises many of the Joystick library functions when pin A0 is grounded.
- `MultipleJoystickTest` - Creates 4 Joysticks using the library and exercises the first 16 buttons and X and Y axis when pin A0 is grounded.
- `JoystickButton` - Creates a Joystick and maps pin 9 to button 0, pin 10 to button 1, pin 11 to button 2, and pin 12 to button 3.
- `JoystickKeyboard` - Creates a Joystick and a Keyboard. Maps pin 9 to Joystick Button 0, pin 10 to Joystick Button 1, pin 11 to Keyboard key 1, and pin 12 to Keyboard key 2.
- `DrivingControllerTest` - Creates a Driving Controller and tests 4 buttons, the Steering, Brake, and Accelerator when pin A0 is grounded.  
- `FlightControllerTest` - Creates a Flight Controller and tests 32 buttons, the X and Y axis, the Throttle, and the Rudder when pin A0 is grounded.
- `HatSwitchTest` - Creates a joystick with two hat switches. Grounding pins 4 - 11 cause the hat switches to change position.  

###Simple example

	#include <Joystick.h>
	
	// Create the Joystick
	Joystick_ Joystick;
	
	// Constant that maps the phyical pin to the joystick button.
	const int pinToButtonMap = 9;
	
	void setup() {
	  // Initialize Button Pins
	  pinMode(pinToButtonMap, INPUT_PULLUP);
	
	  // Initialize Joystick Library
	  Joystick.begin();
	}
	
	// Last state of the button
	int lastButtonState = 0;
	
	void loop() {
	
	  // Read pin values
      int currentButtonState = !digitalRead(pinToButtonMap);
	  if (currentButtonState != lastButtonState)
	  {
	    Joystick.setButton(0, currentButtonState);
	    lastButtonState = currentButtonState;
	  }
	
	  delay(50);
	}

##Joystick Library API
The following API is available if the Joystick library in included in a sketch file.

### Joystick\_(...)
Constructor used to initialize and setup the Joystick. The following optional parameters are available:

- `uint8_t hidReportId` - Default: `0x03` - Indicates what the joystick's HID report ID should be. This value must be unique if you are creating multiple instances of Joystick. Do not use `0x01` or `0x02` as they are used by the built-in Arduino Keyboard and Mouse libraries.
- `uint8_t buttonCount` - Default: `32` - Indicates how many buttons will be available on the joystick.
- `uint8_t hatSwitchCount` - Default: `2` - Indicates how many hat switches will be available on the joystick. Range: `0` - `2`
- `bool includeXAxis` - Default: `true` - Indicates if the X Axis is available on the joystick.
- `bool includeYAxis` - Default: `true` - Indicates if the Y Axis is available on the joystick.
- `bool includeZAxis` - Default: `true` - Indicates if the Z Axis (in some situations this is the right X Axis) is available on the joystick.
- `bool includeRxAxis` - Default: `true` - Indicates if the X Axis Rotation (in some situations this is the right Y Axis) is available on the joystick.
- `bool includeRyAxis` - Default: `true` - Indicates if the Y Axis Rotation is available on the joystick.
- `bool includeRzAxis` - Default: `true` - Indicates if the Z Axis Rotation is available on the joystick.
- `bool includeRudder` - Default: `true` - Indicates if the Rudder is available on the joystick.
- `bool includeThrottle` - Default: `true` - Indicates if the Throttle is available on the joystick.
- `bool includeAccelerator` - Default: `true` - Indicates if the Accelerator is available on the joystick.
- `bool includeBrake` - Default: `true` - Indicates if the Brake is available on the joystick.
- `bool includeSteering` - Default: `true` - Indicates if the Steering is available on the joystick.

The following constants define the default values for the constructor parameter's listed above:

- `JOYSTICK_DEFAULT_REPORT_ID` is set to `0x03`
- `JOYSTICK_DEFAULT_BUTTON_COUNT` is set to `32`
- `JOYSTICK_DEFAULT_HATSWITCH_COUNT` is set to `2`
		
### Joystick.begin(bool initAutoSendState)
Starts emulating a game controller connected to a computer. By default all methods update the game controller state immediately. If `initAutoSendState` is set to `false`, the `Joystick.sendState` method must be called to update the game controller state.

### Joystick.end()
Stops the game controller emulation to a connected computer.

### Joystick.setXAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the X axis. Default: `0` to `1023`

### Joystick.setXAxis(byte value)
Sets the X axis value. See `setXAxisRange` for the range.

### Joystick.setYAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Y axis. Default: `0` to `1023`

### Joystick.setYAxis(byte value)
Sets the Y axis value. See `setYAxisRange` for the range.

### Joystick.setZAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Z axis. Default: `0` to `1023`

### Joystick.setZAxis(byte value)
Sets the Z axis value. See `setZAxisRange` for the range.

### Joystick.setRxAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the X axis rotation. Default: `0` to `1023`

### Joystick.setRxAxis(int value)
Sets the X axis rotation value. See `setRxAxisRange` for the range.

### Joystick.setRyAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Y axis rotation. Default: `0` to `1023`

### Joystick.setRyAxis(int value)
Sets the Y axis rotation value. See `setRyAxisRange` for the range.

### Joystick.setRzAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Z axis rotation. Default: `0` to `1023`

### Joystick.setRzAxis(int value)
Sets the Z axis rotation value. See `setRzAxisRange` for the range.

### Joystick.setRudderRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Rudder. Default: `0` to `1023`

### Joystick.setRudder(byte value)
Sets the Rudder value. See `setRudderRange` for the range.

### Joystick.setThrottleRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Throttle. Default: `0` to `1023`

### Joystick.setThrottle(byte value)
Sets the Throttle value. See `setThrottleRange` for the range.

### Joystick.setAcceleratorRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Accelerator. Default: `0` to `1023`

### Joystick.setAccelerator(byte value)
Sets the Accelerator value. See `setAcceleratorRange` for the range.

### Joystick.setBrakeRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Brake. Default: `0` to `1023`

### Joystick.setBrake(byte value)
Sets the Brake value. See `setBrakeRange` for the range.

### Joystick.setSteeringRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Steering. Default: `0` to `1023`

### Joystick.setSteering(byte value)
Sets the Steering value. See `setSteeringRange` for the range.

###Joystick.setButton(byte button, byte value)
Sets the state (`0` or `1`) of the specified button (range: `0` - (`buttonCount - 1`)). The button is the 0-based button number (i.e. button #1 is `0`, button #2 is `1`, etc.). The value is `1` if the button is pressed and `0` if the button is released.

###Joystick.pressButton(byte button)
Press the indicated button (range: `0` - (`buttonCount - 1`)). The button is the 0-based button number (i.e. button #1 is `0`, button #2 is `1`, etc.).

###Joystick.releaseButton(byte button)
Release the indicated button (range: `0` - (`buttonCount - 1`)). The button is the 0-based button number (i.e. button #1 is `0`, button #2 is `1`, etc.).

###Joystick.setHatSwitch(byte hatSwitch, int value)
Sets the value of the specified hat switch. The hatSwitch is 0-based (i.e. hat switch #1 is `0` and hat switch #2 is `1`). The value is from 0° to 360°, but in 45° increments. Any value less than 45° will be rounded down (i.e. 44° is rounded down to 0°, 89° is rounded down to 45°, etc.). Set the value to `JOYSTICK_HATSWITCH_RELEASE` or `-1` to release the hat switch.

###Joystick.sendState()
Sends the updated joystick state to the host computer. Only needs to be called if `AutoSendState` is `false` (see `Joystick.begin` for more details).

##Testing Details
I have used this library to make an Arduino appear as the following:

- 1 joystick / gamepad
- 2 joysticks / gamepads
- 3 joysticks / gamepads
- 4 joysticks / gamepads

I have tested with 1 - 32 buttons.

I have tested with 0, 1, and 2 hat switches.

I have tested this library using the following Arduino IDE Versions:

- 1.6.6
- 1.6.7
- 1.6.8
- 1.6.9

I have tested this library with the following boards:

- [Arduino Leonardo](https://www.arduino.cc/en/Main/ArduinoBoardLeonardo)
- [Arduino Micro](https://www.arduino.cc/en/Main/ArduinoBoardMicro)

Others have tested this library with the following boards:
- [SparkFun Pro Micro](https://www.sparkfun.com/products/12640)

Other board notes:
- [Arduino Due](https://www.arduino.cc/en/Main/ArduinoBoardDue) - I am told that the Arduino IDE 1.6.5 (and below) version of this library (see [Add USB Game Controller to Arduino Leonardo or Micro](http://mheironimus.blogspot.com/2015/03/add-usb-game-controller-to-arduino.html)) worked with the Arduino Due, but the new Arduino Joystick Library (i.e. both version 1.x and version 2.x) does not work with Arduino Due at this time.
- [Arduino UNO](https://www.arduino.cc/en/Main/ArduinoBoardUno) - NOT Supported - However, it might work with the [NicoHood/HoodLoader2](https://github.com/NicoHood/HoodLoader2) library, but I have not had a chance to try this out yet.
- [Arduino MEGA](https://www.arduino.cc/en/Main/ArduinoBoardMega2560) - NOT Supported - However, it might work with the [NicoHood/HoodLoader2](https://github.com/NicoHood/HoodLoader2) library, but I have not had a chance to try this out yet.


(as of 2016-06-24)
