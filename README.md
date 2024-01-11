# RT1_assignment2


Second Assignment description
===================
The task of this assignment was to implement three new nodes in the robot simulation:

* A node (a) that implements an action client, allowing the user to set a target (x, y) or to cancel it.The node also publishes the robot position and velocity as a custom message (x,y, vel_x, vel_z), by relying on the values published on the topic /odom
* A service node (b) that, when called, returns the coordinates of the last target sent by the user;
* Another service node (c) that subscribes to the robot’s position and velocity (using the custom message) and implements a server to retrieve the distance of the robot from the target and the robot’s average speed.

It is also required to create a launch file (assignment1.launch) to start the whole simulation using a parameter to select the size of the averaging window of node (c).



How to run the code:
-----------------------

Go inside the root directory of your ROS workspace and run the command:

> catkin_make

Now we need to run the ROS master in a separete terminal:

> roscore

Then, launch the simulation with the roslaunch command:

> roslaunch assignment_2_2023 assignment1.launch

Finally, start each node with these commands:

>rosrun assignment_2_2023 node_a.py

>rosrun assignment_2_2023 node_b.py

>rosrun assignment_2_2023 node_c.py


Useful commands:
------------------

If you want to get the coordinates of the last target, run the command:

> rosservice call /get_last_target

If you want to know the distance of the robot from the target and the robot’s average speed, run the command: 

> rosservice call /get_robot_stats "{target: {x:, y: }}"

Inside the "assignmet1.launch" launch file, you can set the value for the frequency with which node (c) publishes the information.

> param name="publish_frequency" type="double" value="number_value" 

Action client's pseudocode [node (a)]:
----------------------------

1. Run the node
2. Initialize ROS node
3. Create an object of class "ActionClient"
4. Within the class "Client":
   * Initialization function "__init__":
      - Initialize ROS node
      - Create a publisher for robot position information
      - Create action client for reaching_goal action
      - Wait for the server to be ready
      - Create a publisher for target coordinates
      - Create a subscriber to listen for /odom topic
      - Create a goal message
   * Implement the "__odom_callback__" function:
      - Extract position and linear velocity information from odometry message
      - Create custom message to publish position and velocity information
      - Publish custom message
   * Implement the "__set_goal__" function:
      - Set goal coordinates in action goal message
      - Send goal to action sever
      - Publish target coordinates
   * Implement the "__cancel_goal__" function:
      - Cancel the current goal
      - Log the information about the goal cancellation 
5. Get user input for goal coordinates or cancel it
  * If the user inputs 'c', call the "__cancel_goal__" function
  * If the user inputs the new goal
    - parse the user input as float values for x and y
    - call the "__set_goal__" function to set the goal using the parsed coordinates
  * If the user inputs in an invalid format, log a warning
6. Spin to keep the ROS node alive

  
   

