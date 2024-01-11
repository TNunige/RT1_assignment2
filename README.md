# RT1_assignment2
assignment2_2023
===================

In this assignment we are going to learn how to operate ROS costum messages, costum services, and actions. We are also going to use the graphical interfaces (Rviz and Gazebo) to view the robot's simulation. The new package was built starting from the template provided, assignment_2_2023.

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
2. Initialize a node named "client" in ROS
3. Create an object of class "Client"
4. Within the class "Client":
   * Initialization function "__init__":
      - Set two variables "position" and "linear_velocity" to None
      - Create a publisher to publish position information
      - Create a subscriber to listen for odometry information and store the received position and linear velocity information
      - Create an action client for the "reaching_goal" action server
      - Wait for the action server to become usable
      - Create a goal message for the "reaching_goal" action
      - Create a publisher to send the target
   * Implement the "odom_callback" function:
      - Store the received position and linear velocity information from the odometry message
      - Create a new message to publish the position and velocity information
      - Publish the message
   * Implement the "goal_input" function:
      - Print a prompt to ask the user to insert the goal coordinates or cancel the goal
      - Listen for user input in an infinite loop
      - If the user inputs 'c', cancel the current goal
      - If the user inputs is a float value, set the goal coordinates and send the goal to the action server and to the message
      - If the input is not a float value, print an error message
5. Call the "goal_input" function with using the object "client"

