# Overview

This repository contains an IEC 61499 system that simulates control of two motors. The system's objective is to allow only one motor to run at a time. There's an assert in place that if both motors are running at the same time, the system stops. The motors are represented by OPC UA variables, as are the inputs to the system that start and stop them.

An application simulating the user's input starting and stopping the motors is also present, and can be used to trigger the controller. 

Traces are provided in the `traces` folder, which can be used to replay debug the controller's application. The existing traces contain an example which demostrates that the system contains a bug. Use the replay debugging feature to find the bug. Good luck!

# Controller Application

The application consists of two symmetrical groups of function blocks, each controlling one motor (X represents either the first or second motor and Y the other one):
 - `StartMotorX`: input to start (true) and stop (false) the motor.
 - `TurnMotor1OnOff`: Based on the received data (true or false), it triggers the respective event to start or stop the motor.
 - If starting the motor:
     - `IsMotorYOff` and `AllowOnlyIfMotorYOff`: Check if the other motor is currently running, and allows the event to go forward if the other motor is off.
- `Motor1CurrentState`: Holds the boolean information if the motor is now turned on or off.
- `Motor1`: Output of the system, representing the boolean sent to the motor to turn it on or off.

Both symmetrical groups come together into an assert network (`AreBothOn`, `Assert`, `StopApplication`), which checks if both motors are running, and if that's the case, it stops the application. This check is executed every time a motor is turned on or off.

# User Simulator

A simple application simulating different input patterns from the user, which connects to the StartMotorX function blocks from the Controller Application.

# Replay Debugging

This repository's main objective is to demonstrate the capabilities of replay debugging. Check the up-to-date documentation [here](https://eclipse.dev/4diac/doc/tutorials/replaydebugger.html) to understand its capabilities and how to use it.
The traces to be used are present in the `traces` folder.
