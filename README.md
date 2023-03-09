# HomeServiceRobot
Overview:
Developed and built a capstone project for Udacity’s Nano Degree Program in Robotics Software Engineering, designed to autonomously pick and drop off virtual objects by Home Service Robot using three key robotics algorithms: Localization, SLAM, and Path Planning. Implemented AMCL for localization, RTAB-Map package to generate a map, and deployed Dijkstra’s algorithm for obstacle avoidance while picking up and dropping off objects in a ROS simulated environment by coding the nodes in C++.

## Package Explanation:

This project mainly deals with five packages:

1. Gmapping: ROS gmapping is used to automatically build a map of its environment using data from a laser range finder sensor and the robot's odometry. This package is main responsible for autonomous navigation or mapping. Initially the sensor data from robot is collected to extract the feature of the environment like obstacles, walls and objects. Further more creates a 2D occupancy grid map of the environment, where each cell in the grid represents the probability of an obstacle being present in that location. Once a map has been generated, the robot can use it to determine its location within the environment using Adaptive Monte Carlo Localization (AMCL).

2. AMCL: A ROS package for localizing the robot within the environment using a map generated by gmapping. The AMCL package uses sensor data, such as data from a laser range finder or RGB-D camera, to estimate the robot's pose relative to a pre-built map of the environment. The particle filter algorithm used by AMCL works by representing the robot's possible poses as a set of particles, each with its own weight. As the robot moves and gathers sensor data, the particles are updated based on the sensor data and the robot's motion, with the particles that match the sensor data more closely being assigned higher weights. Over a period of time with more sensore data the particles converge to accurate Robot's Pose.

Here AMCL is conjunction with Gmapping package to perform Simultaneous Mapping and Localization (SLAM) in Home Service Robot project. This is one of the powerful SLAM technique used in real-world aplication(indore). 

3. Navigation: A package for autonomous navigation of the robot, including path planning, obstacle avoidance and controlling the motion of a robot to enable it to move from one location to another. In this project ROS Navigation stack is used to provide high level navigation capabilities to the robot.

4. Turtlebot Teleop: A package for manually controlling the turtlebot robot using a keyboard or joystick.

5. Pick Objects: A custom package for the PICKUP and DROPOFF task in this project, which involves navigating to a specified location, picking up an object, and delivering it to another location. This package is subscribed to pick_objects.app to perform the action accordingly. Furthermore pick_objects.sh file is created to connect the world, map and code together.


6. Add Markers: This node is a custom ROS package that is used to simulate the virtual markers for pickup and drop-off of objects by the robot. The "add_markers" node subscribes to the robot's odometry topic and displays a marker in RViz at a PICKUP location, representing the object that the robot needs to pick up. Once the robot reaches the pickup location, the "add_markers" node hides the marker, simulating the pickup of the object. Likewise, the marker displays at DROPOFF location representing the object is delivered. Furthermore add_markers.sh file is created to connect the world, map and code together.

7. Home Service Robot: This package connects all the other tiny packages to enable the Robot to traverse towards PICKUP location and wait for five seconds to pick the virtual marker and then traverse towards DROPOFF location. Additionally, Dijkstra's algorithm (a variant of the Uniform Cost Search algorithm) is used to avaoid collision along the path. Finally the marker is left at Drop location for infinite time. Furthermore home_serv_robot.sh file is created to connect the world, map, packages and code together.

Overall, these packages provide a range of capabilities for autonomous navigation, mapping, and localization in the Udacity Home Service Robot project, further more these algorithms are used in real-world robotics applications as well.


## Result:
Overall, the robot generates a map using and gmapping and uses AMCL to localize withoin the environment. Now, the Robot uses all the respective packages to PICKUP and DROPOFF virtual marker in their respective locations. All the simulations are succesfully submitted and the Robot perform extremely well in complex environment by avoiding obstacles and traverse to its goal by using Dijkstra's algorithm.


## Prerequisites/Dependencies  
* Gazebo >= 7.0  
* ROS Kinetic  
* ROS navigation package  
```
sudo apt-get install ros-kinetic-navigation
```
* ROS map_server package  
```
sudo apt-get install ros-kinetic-map-server
```
* ROS move_base package  
```
sudo apt-get install ros-kinetic-move-base
```
* ROS amcl package  
```
sudo apt-get install ros-kinetic-amcl
```
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
  
  
## Setup Instructions (abbreviated)  
1. Meet the `Prerequisites/Dependencies`  
2. Open Ubuntu Bash and clone the project repository  
3. On the command line and execute  
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
4. On the command line and execute  
```
cd RoboND-Term1-P5-Home-Service-Robot/catkin_ws/src  
git clone https://github.com/ros-perception/slam_gmapping.git  
git clone https://github.com/turtlebot/turtlebot.git  
git clone https://github.com/turtlebot/turtlebot_interactions.git  
git clone https://github.com/turtlebot/turtlebot_simulator.git  
```

5. Build and run your code.  

## Project Description  
Directory Structure  
```
.Home-Sevice-Robot                                        # Home Service Robot Project
├── catkin_ws                                             # Catkin workspace
│   ├── src
│   │   ├── add_markers                                   # add_markers package        
│   │   │   ├── launch
│   │   │   │   ├── view_home_service_navigation.launch   # launch file for home service robot demo
│   │   │   ├── src
│   │   │   │   ├── add_markers.cpp                       # source code for add_markers node
│   │   │   │   ├── add_markers_demo.cpp                  # source code for add_markers_demo
│   │   ├── pick_objects                                  # pick_objects package     
│   │   │   ├── src
│   │   │   │   ├── pick_objects.cpp                      # source code for pick_objects node
│   │   │   │   ├── pick_objects_demo.cpp                 # source code for pick_objects_demo
│   │   ├── rvizConfig                                    # rvizConfig package        
│   │   │   ├── home_service_rvizConfig.rviz              # rvizConfig file for home service robot demo  
│   │   ├── scripts                                       # shell scripts files
│   │   │   ├── add_marker.sh                             # shell script to model virtual objects  
│   │   │   ├── home_service.sh                           # shell script to launch home service robot demo  
│   │   │   ├── pick_objects.sh                           # shell script to send multiple goals  
│   │   │   ├── test_navigation.sh                        # shell script to test localization and navigation
│   │   │   ├── test_slam.sh                              # shell script to test SLAM
│   │   ├── slam_gmapping                                 # gmapping_demo.launch file
│   │   ├── turtlebot                                     # keyboard_teleop.launch file
│   │   ├── turtlebot_interactions                        # view_navigation.launch file
│   │   ├── turtlebot_simulator                           # turtlebot_world.launch file package        
│   │   ├── CMakeLists.txt                                # compiler instructions
├── video.mp4                                             # Videos for overview
├── video.gif                                             # GIF for overview
```

- [view_home_service_navigation.launch](/catkin_ws/src/add_markers/launch/view_home_service_navigation.launch): Launch rviz with specify rviz configuration file  
- [add_markers.cpp](/catkin_ws/src/pick_objects/src/add_markers.cpp): C++ script, communicate with `pick_objects` node and control the marker appearance to simulate object pick up and drop off   
- [pick_objects.cpp](/catkin_ws/src/pick_objects/src/pick_objects.cpp): C++ script, communicate with `add_markers` node and command the robot to pick up the object  
- [home_service_rvizConfig.rviz](/catkin_ws/src/rvizConfig/home_service_rvizConfig.rviz): rvizConfig file for home service robot demo which contained `markers` option  
- [add_marker.sh](/catkin_ws/src/scripts/add_marker.sh): Shell script file to deploy a turtlebot inside your environment, model a virtual object with markers in `rviz`.  
- [home_service.sh](/catkin_ws/src/scripts/home_service.sh): Shell script file to deploy a turtlebot inside your environment, simulate a full home service robot capable of navigating to pick up and deliver virtual objects.  
- [pick_objects.sh](/catkin_ws/src/scripts/pick_objects.sh): Shell script file to deploy a turtlebot inside your environment, communicate with the ROS navigation stack and autonomously send successive goals for your robot to reach.  
- [test_navigation.sh](/catkin_ws/src/scripts/test_navigation.sh): Shell script file to deploy a turtlebot inside your environment, pick two different goals and test your robot's ability to reach them and orient itself with respect to them.  
- [test_slam.sh](/catkin_ws/src/scripts/test_slam.sh): Shell script file to deploy a turtlebot inside your environment, control it with keyboard commands, interface it with a SLAM package, and visualize the map in `rviz` 

- [CMakeLists.txt](/catkin_ws/src/CMakeLists.txt): File to link the C++ code to libraries.  
- [video.gif](video.gif): A gif of video for final home service robot run  

## Run the project  
* Clone this repository
```
git clone https://github.com/daddalasaipraveen/HomeServiceRobot-PickandPlace-.git
```
* Navigate to the `src` folder and clone the necessary repositories  
```
cd RoboND-Term1-P5-Home-Service-Robot/catkin_ws/src  
Follow the above instructions to clone slam_gmapping, turtlebot, turtlebot_interactions, turtlebot_simulator
```
* Open the repository, make and source  
```
cd /home/workspace/catkin_ws/
catkin_make
source devel/setup.bash
```
* Launch the home service robot
```
./src/scripts/home_service.sh
```
* Done. 

## Tips  
1. It's recommended to update and upgrade your environment before running the code.  
```
sudo apt-get update && sudo apt-get upgrade -y
```
2. If your system python version from miniconda is python3 while the ros packages and tf are python2. A hack is to just set the system python to python2 via symbol link. Run the following commands to resolve it  
```
ln -s /usr/bin/python2 /root/miniconda3/bin/python
```
3. How to setup your environment at start up.  
```
Add the following line into the /home/workspace/.student_bashrc
export PYTHONPATH=$PYTHONPATH:/usr/lib/python2.7/dist-packages  
pip install catkin_pkg  
pip install rospkg  
```
4. How create package with dependencies  
```
catkin_create_pkg pick_objects move_base_msgs actionlib roscpp  
catkin_create_pkg add_markers roscpp visualization_msgs  
```
5. How to visualize your marker in the rviz  
To see the marker(virtual objects) demo, in addition to running the `./add_marker.sh`, you will need to manually add a 'Marker' in rviz with the following steps:  
* Find your rviz window  
* In the left bottom panel, click "Add" button  
* In 'By display type' tab, navigate the tree to 'rviz' then 'Marker'  
* Click 'OK' button  
* Done, you should see the marker(virtual objects) appear, disappear then appear again  

## Code Style  
Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Glimpse of home service robot that Pick objects and Place accordingly 

![homeServiceRobot](https://user-images.githubusercontent.com/89826843/224058504-98e0e161-c03e-4acd-9b09-a0eeec256837.gif)

![Screenshot (5)](https://user-images.githubusercontent.com/89826843/224061284-b79face9-71fc-48b9-ab74-698b229c7ded.png)
![Screenshot (6)](https://user-images.githubusercontent.com/89826843/224061296-5bd7bada-3790-4660-b65c-cb3b1a216a8d.png)
![Screenshot (7)](https://user-images.githubusercontent.com/89826843/224061319-79a8d207-1f5f-4667-bcb5-f3c7aabbb48b.png)
![Screenshot (8)](https://user-images.githubusercontent.com/89826843/224061338-db608e72-d48c-4f47-8110-d529c9ca5e05.png)
![Screenshot (9)](https://user-images.githubusercontent.com/89826843/224061352-51500cb4-d398-4db0-aaf2-cb7f7097197a.png)
![Screenshot (10)](https://user-images.githubusercontent.com/89826843/224061367-34d483c3-e66c-4822-83d6-2b56dfe50299.png)
![Screenshot (11)](https://user-images.githubusercontent.com/89826843/224061379-1e5dca49-5339-4e9f-afb7-e25ecb1ab140.png)
![Screenshot (12)](https://user-images.githubusercontent.com/89826843/224061392-86d087d0-c65c-4a70-abb8-92a6b625c9c5.png)
![Screenshot (13)](https://user-images.githubusercontent.com/89826843/224061410-b51c2a59-6cbb-4b5e-9abf-688ea02252e2.png)


