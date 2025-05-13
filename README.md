![image](https://github.com/user-attachments/assets/1f4e2836-7387-40b6-8596-773b8c4b9138)1. Serial port command query
The official ROS car of Lunqu has a serial port control interface, which is mentioned in the instruction document attached to the car:

Frame header: fixed value Ox7B, marking the beginning of the data packet, occupying one byte;
Reserved bit: The second and third bytes are reserved bits, meaningless, each occupying one byte.
X-axis target speed: target speed on the robot's X-axis, unit.mm/s, occupying two bytes;

Y-axis target speed: target speed on the robot's Y-axis, unit mm/s, occupying two bytes;

Z-axis target speed: target speed on the robot's Z-axis magnified 1000 times, unit.rad/s, occupying two bytes;
Data check bit: the result of the first 9 bytes XOR check (BCC check), occupying one byte;Frame tail: fixed value Ox7D, marking the end of the data packet, occupying one byte;

The baud rate of Lunqu's motherboard is generally 115200, you can also look at the motherboard source code
The check digit can be calculated through the web page:[BCC校验(异或校验)在线计算_ip33.com](http://www.ip33.com/bcc.html)

The serial port location can be found in the relevant information provided.

2.Interpretation of functions in project files
Mainly look at the function I wrote, which was written as a library function:

The main function mainly realizes moving forward for 1s and turning left for 1s (continuous loop).

order.c is a motion instruction function library, in which forward is my custom function. forward(1000) means to move forward 1000ms. These interfaces are written into order.c by me.
The main functions used in order.h are the forward, backward, right, and left motion instructions. Of course, left_low corresponds to a low-speed left turn, and stop() is an empty function that disables all motors.

map.c map function: If you want to follow a specific track, you can integrate all the movement instructions in map.c. Here, the switch statement is used to ensure the order of action execution to achieve high-precision fixed track movement.

up_sensor.c external sensor function: The sensor here is my external ultrasonic sensor, which detects obstacles in front. I did not connect it to the main board of the wheel, but to my stm32 host computer. If you do not connect it externally, you can comment out the sensor-related code. If you want to modify the sensor interface, you can modify it in sup_sensor.c



​
