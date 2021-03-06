---------------------------------------------------------------------------------------------------------------------

by Batuhan Alkan submitted on August 2021

---------------------------------------------------------------------------------------------------------------------



# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Simulator.
You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).  

To run the simulator on Mac/Linux, first make the binary file executable with the following command:
```shell
sudo chmod u+x {simulator_file_name}
```

### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

#### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

Here is the data provided from the Simulator to the C++ Program

#### Main car's localization Data (No Noise)

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH

#### Previous path data given to the Planner

//Note: Return the previous list but with processed points removed, can be a nice tool to show how far along
the path has processed since last time. 

["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator

#### Previous path's end s and d values 

["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

#### Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

## Details

1. The car uses a perfect controller and will visit every (x,y) point it recieves in the list every .02 seconds. The units for the (x,y) points are in meters and the spacing of the points determines the speed of the car. The vector going from a point to the next point in the list dictates the angle of the car. Acceleration both in the tangential and normal directions is measured along with the jerk, the rate of change of total Acceleration. The (x,y) point paths that the planner recieves should not have a total acceleration that goes over 10 m/s^2, also the jerk should not go over 50 m/s^3. (NOTE: As this is BETA, these requirements might change. Also currently jerk is over a .02 second interval, it would probably be better to average total acceleration over 1 second and measure jerk from that.

2. There will be some latency between the simulator running and the path planner returning a path, with optimized code usually its not very long maybe just 1-3 time steps. During this delay the simulator will continue using points that it was last given, because of this its a good idea to store the last points you have used so you can have a smooth transition. previous_path_x, and previous_path_y can be helpful for this transition since they show the last points given to the simulator controller with the processed points already removed. You would either return a path that extends this previous path or make sure to create a new path that has a smooth transition with this last path.

## Tips

A really helpful resource for doing this project and creating smooth trajectories was using http://kluge.in-chemnitz.de/opensource/spline/, the spline function is in a single hearder file is really easy to use.

---

## Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Rubric
**Compilation**

**1-The code compiles correctly.**


![](/bild1.png)

**2- Valid trajectories**

I ran the simulator for 6.70 miles without an incident.

**3- Speed and Acceleration Limits**

No speed limit red message was seen.

Max jerk message was not seen.

Car does not have collisions.

**4- Staying in the Lane**

The car stays in its lane most of the time, but may change lanes or stop in the middle lane due to traffic.


![](/gerade.gif)



**5- Lane Change**

The car can easily change lanes when it makes sense, e.g. B. behind a slower moving car and when there is no other traffic in a neighboring lane.


![](/uberholen.gif)

## Reflection

The path planning algorithms and Sensor Fusion algorithms start at src/main.cpp. A helper function (spline.h) is added for this project.
 
***The Path Planning module is broken down into the following set of 3 sub-modules:***

![](/bild2.png)

  -  **Prediction:** Prediction of the trajectories of the surrounding detected objects.
 in this Project **Lines between 69-152** in main.cpp commense with to get data from sensor fusion including localization data in order to predict the positions for the surrounding cars.It basically checks whether there are cars in other lanes. This step is critical to avoid collision.Each lane is divided into 3 segments of 30 m and each segment contains information about the vehicle speed and the vehicle value. The road matrix is filled with corresponding values from sensor fusion data so that the behavior planning module can see the road situation for 90 m ahead for 3 lanes and plan the vehicle behavior accordingly. A 3x3 matrix was used for simplicity. A larger matrix with shorter segments (like 5m) depicting a road situation around the car could be used to produce more advanced behavior.
 
  - **Behavior:** Defines a set of possible high-level targets for the tool to follow. In this Project **Lines 154-184** in main.cpp determine whether the ego car should accelerate / brake or change lanes. The reference speed is also set here in order to prevent the speed limit from being exceeded. When a car is detected in the same lane up to 30 meters ahead, it checks the 1st and 2nd segments of other lanes to find the lane with less traffic. When all lanes are occupied, the vehicle reduces its speed and holds the lane waiting while the adjacent lane is available again. If the distance quickly decreases to 15 m or less, the behavior planner applies an ???emergency brake??? to avoid a possible collision.
 
   - **Trajectory:** An exact path is calculated for each possible high-level target. For each trajectory, costs (based on feasibility, safety, legality, convenience, and efficiency) are derived and the trajectory with the lowest cost is selected. In this Project **Lines 187 through 295 in main.cpp**are the orbital creation part. This section essentially creates the trajectories using the output of the behavior planning section. It is very important that the trajectory created is smooth. Otherwise, undesirable accelerations can occur. For this purpose, past trajectory points are copied and waypoints filled with splines. The acceleration depends on each point on the trajectory to maintain dynamic decision making as traffic conditions can change abruptly. Therefore, the ego car can change its speed when changing lanes.
 
 
 ![](/seiten.gif)
 
 
## Coordinate transforms

  <p align="center">
<img src=".//bild3.png">
</p> 




Before going into the main.cpp file details, discuss process models, mentioning "Frenet coordinates", which are a more intuitive representation of the position on a street than traditional (x, y) Cartesian coordinates. With Frenet coordinates, the variables s and d are used to describe the position of a vehicle on the road. The s-coordinate represents the distance along the road (also known as longitudinal displacement) and the d-coordinate represents the lateral position on the road.





 ![](/oben.gif)
