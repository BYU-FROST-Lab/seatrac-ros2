## Using with ROS2

For applications using ros, the ros2 seatrac node provides a high level interface
with the beacon. 

the ros2_seatrac_ws workspace contains 3 packages:

* seatrac - a package containing a node to communicate with the seatrac modem
* py_pinger - a package with a node that sends pings using the seatrac node
* seatrac_interfaces - A package with 4 interfaces:
	* ModemSend   -  instructs the beacon to send acoustic messages.
	* ModemRec    -  returned when an acoustic signal is received.
	* ModemStatus -  returned when a status message is received.
	* ModemCmdUpdate - captures short status and error codes

The ros2 seatrac node is designed mainly to send and interpret acoustic transmissions. 
It does not support changing settings, setting beacon id, calibration, or diagnostics. 
These functions can be achieved using the c++ interface.

#### To run ROS2:

1. Source your local ros2 distibution `source /opt/ros/<distro>/setup.bash`.
	The ros disto this workspace was developed on is Humble.
1. Navigate to `seatrac_driver/tools/ros2_seatrac_ws`
2. Build the workspace and source local setup: `colcon build && source ./install/setup.bash`
3. Launch: `ros2 launch launch.py`. This launches the seatrac and python pinger nodes,
	and executes 'ros2 topic echo /modem_rec'. To run the seatrac node alone, execute
	`ros2 run seatrac modem`.

#### Reading messages from the beacon (ros2)

The three ros2 messages published from the seatrac beacon are modem_rec, modem_status, and modem_cmd_update. ModemRec is published anytime the beacon receives an acoustic message and includes directional and ranging information. ModemStatus is published anytime the driver recieves a CID_STATUS update from the beacon. ModemCmdUpdate is published anytime the beacon receives an error message or command status update.

Links: [ModemRec.msg](https://bitbucket.org/frostlab/seatrac_driver/src/main/tools/ros2_seatrac/seatrac_interfaces/msg/ModemRec.msg), [ModemStatus.msg](https://bitbucket.org/frostlab/seatrac_driver/src/main/tools/ros2_seatrac/seatrac_interfaces/msg/ModemStatus.msg), [ModemCmdUpdate.msg](https://bitbucket.org/frostlab/seatrac_driver/src/main/tools/ros2_seatrac/seatrac_interfaces/msg/ModemCmdUpdate.msg)

#### Sending acoustic transmission commands (ros2)

ModemSend allows you to send commands to the beacon that instruct
the beacon to transmit an acoustic message. The field msg_id is the CID_E of the message
and can only take 4 values: CID_PING_SEND, CID_DAT_SEND, CID_ECHO_SEND, or CID_NAV_QUERY_SEND.

Link to ModemSend: [ModemSend.msg](https://bitbucket.org/frostlab/seatrac_driver/src/main/tools/ros2_seatrac/seatrac_interfaces/msg/ModemSend.msg)

