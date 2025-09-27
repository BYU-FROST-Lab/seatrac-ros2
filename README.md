# ROS2 SeaTrac X110/X150 Driver

A driver to use the SeaTrac X110/X150 Acoustic USBL Beacon. This repository contains two packages: seatrac and seatrac_interfaces.

Built on the C++ seatrac driver found [here](https://github.com/BYU-FRoSt-Lab/seatrac_driver).


## Installation

seatrac and seatrac_interfaces are standard ros2 packages. Clone this repository into your ros2 workspace and use colcon to build.

## Overview

* seatrac - A package to interface with the seatrac beacon. It contains two nodes and an executable
	* modem_ros_node: The main node that interfaces with the beacon.  Call `ros2 run seatrac modem` to run.
	* modem_pinger: A node that repeatedly sends an acoustic ping to another beacon on a timer.  Call `ros2 run seatrac modem_pinger` to run.
	* seatrac_calibration_and_settings: A terminal program to change settings and calibrate a beacon before launch. Call `ros2 run seatrac calibration_and_settings` to use.

* seatrac_interfaces - A package with 4 interfaces:
	* ModemSend   -  instructs the beacon to send acoustic messages.
	* ModemRec    -  returned when an acoustic signal is received. 
	* ModemStatus -  returned when a status message is received.
	* ModemCmdUpdate - captures short status and error codes.

## Usage

1. Source your local ros2 distibution `source /opt/ros/<distro>/setup.bash`.
2. Build the workspace and source local setup: `colcon build && source ./install/setup.bash`
3. Call `ros2 run seatrac modem` to talk to beacon. 

## Reading messages from the beacon

The three ros2 topics published from the seatrac beacon are modem_rec, modem_status, and modem_cmd_update. ModemRec is published anytime the beacon receives an acoustic message and includes directional and ranging information. ModemStatus is published anytime the driver recieves a CID_STATUS update from the beacon. ModemCmdUpdate is published anytime the beacon receives an error message or command status update.

## Sending acoustic messages

Use the modem_send topic to send commands to the beacon that instruct the beacon to transmit an acoustic message. The field msg_id is the CID_E of the message and can only take 4 values: CID_PING_SEND, CID_DAT_SEND, CID_ECHO_SEND, or CID_NAV_QUERY_SEND.

## SeaTrac Support Webpage

https://www.blueprintsubsea.com/seatrac/support

