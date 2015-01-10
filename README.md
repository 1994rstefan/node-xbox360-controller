# Node Xbox Controller
This Modul access the Xbox 360 controller directly via USB.

# Limitations
* Only the Xbox 360 wireless controller with the Xbox 360 wireless receiver is currently supported
* You have to run the application as root, otherwise you can not access the USB device
* Only tested on Linux (Arch 64 Bit) may currently not work with other distributions or operating systems

# Installation
Download the "node-xbox360-controller.js" file and put it in your application directory.
Load it with "XboxController = require('./node-xbox360-controller.js')".
A NPM package will follow if the module is stable and tested enough.

# Dependencies
Only the "usb" npm package is required in order to access the USB device
Package and install instructions: https://www.npmjs.com/package/usb

# Usage
* Require the module
> var XboxController = require('./node-xbox360-controller.js');
* Initialize the controller, use a number between 0 and 3 as controllerId, 0 is the first controller (Player 1)
> XboxController.init(controllerId);
* Use the controller
> XboxController[controllerId]...

# Controller functions
* getData() - Return all information of the controller (see Controller status for more information)
* setRumble(large, small) - Set the rumbling of the large and small motor, use a number between 0 and 255 as parameter
* setLed(state) - Set the LED (see Controller LED states for available states)
* turnOff() - Turn off the controller
* Developer/Expert functions (you should never need to use them)
    * requestStatusUpdate() - Request the controller to send its current status
    * writeRaw(buffer) - Send the data in buffer to the controller

# Controller status
The controller status is handled by an object with the following keys.
* connection - number (0 = no controller connected, 1 = headset connected, 2 = controller connected, 3 = headset and controller connected)
* ready - boolean (false when there is no controller connected or the controller is starting, true when the controller is ready for use)
* rechargeable - boolean (false when normal batteries are used, true for rechargeable batteries / Play'n'Charge Kit)
* battery - number (value between 0 and 3, 0 means empty, 3 means full)
* state - states of the buttons, sticks and triggers
    * btnDPadUp - boolean (true when button is pressed)
    * btnDPadDown - boolean (true when button is pressed)
    * btnDPadLeft - boolean (true when button is pressed)
    * btnDPadRight - boolean (true when button is pressed)
    * btnA - boolean (true when button is pressed)
    * btnB - boolean (true when button is pressed)
    * btnX - boolean (true when button is pressed)
    * btnY - boolean (true when button is pressed)
    * btnBack - boolean (true when button is pressed)
    * btnXbox - boolean (true when button is pressed)
    * btnStart - boolean (true when button is pressed)
    * btnStickLeft - boolean (true when button is pressed)
    * btnStickRight - boolean (true when button is pressed)
    * btnBumperLeft - boolean (true when button is pressed)
    * btnBumperRight - boolean (true when button is pressed)
    * stickLeft - object (x and y axis)
        * x - number (-32768 = complete left / 0 = center / +32767 = complete right)
        * y - number (-32768 = complete down / 0 = center / +32767 = complete up)
    * stickRight - object (x and y axis)
        * x - number (-32768 = complete left / 0 = center / +32767 = complete right)
        * y - number (-32768 = complete down / 0 = center / +32767 = complete up)
    * triggerLeft - number (0 = not pressed / 255 = full pressed)
    * triggerRight - number (0 = not pressed / 255 = full pressed)

# Controller LED states
* -1 - automatically determine the correct LED state based on the controllerId (Player 1, Player 2, Player 3 and Player 4)
* 0 - all off
* 1 - all blinking
* 2 - 1 flashes, then on
* 3 - 1 flashes, then on
* 4 - 1 flashes, then on
* 5 - 1 flashes, then on
* 6 - 1 on
* 7 - 2 on
* 8 - 3 on
* 9 - 4 on
* 10 - rotating (1-2-4-3)
* 11 - blinking (The previous setting will be used - all blinking, or 1, 2, 3 or 4 on)
* 12 - slow blinking (The previous setting will be used - all blinking, or 1, 2, 3 or 4 on)
* 13 - alternating (1+4-2+3) (goes back to previous setting after some time)

# Events emitted
Every event callback gets as first parameter the value (see controller status for the available values) and the complete controller status object as second parameter)
See next section for available button, stick and trigger names
* connectionStateChange - Emitted when a controller or headset get connected/disconnected
* ready - Emitted when the controller is ready for usage (controller status gets passed as first parameter, no second parameter available)
* battery - Emitted when the state of the battery changes
* stateChange - Emitted when any input occurs (button press, stick or trigger movement) (controller **state** gets passed as first parameter - only the state sub-oject, not the whole status object)
* change:[ButtonName] - Emitted when the state of a button changes (first parameter is true when the button is now pressed, false otherwise)
* press:[ButtonName] - Emitted when the button gets pressed (first parameter is the same as for "stateChange" event)
* release:[ButtonName] - Emitted when the button gets released (first parameter is the same as for "stateChange" event)
* move:[Stick|Trigger] - Emitted when a stick or trigger gets moved
* moveX:[Stick] - Emitted when a stick moves along the x-axis
* moveY:[Stick] - Emitted when a stick moves along the y-axis

# Button, stick and trigger names
See wikipedia for a naming schema of the buttons - http://commons.wikimedia.org/wiki/File:360_controller.svg
* DPadUp
* DPadDown
* DPadLeft
* DPadRight
* A
* B
* X
* Y
* Back
* Xbox (usually called guide button)
* Start
* StickLeft (as button for change, press and release events)
* StickRight  (as button for change, press and release events)
* BumperLeft
* BumperRight
* StickLeft  (as stick for move, moveX and moveY events)
* StickRight  (as stick for move, moveX and moveY events)
* TriggerLeft
* TriggerRight

# Example usage
See the example.js file for a working example