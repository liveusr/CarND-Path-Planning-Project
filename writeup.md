### Path Planning Project

The goal of this project is to drive a car around the highway with speed limit of 50 MPH. Car should change lanes and pass slower traffic whenever possible. Other cars on road are at speed +/-10 MPH of road speed. 

This is implemented in a simulator based environment. The simulator provides car's localization data. Simulator also provides other cars' data driving on the same side of road. Code from Project Q&A and lessons is used to start with. Model uses six different steps to find out the optimum trajectory. Following sections describe model in detail.

**1. Estimate position of other cars**<br>
Simulator provides sensor fusion data. It is a list of attributes of other cars on the same side of road. This sensor fusion data is processed to estimate distance of each car from our car and closest cars in each lane are calculated. This is implemented in code from [line 106 to 145](https://github.com/liveusr/CarND-Path-Planning-Project/blob/master/src/main.cpp#L106).

**2. Predict best lane to drive**<br>
Once distances of closest cars are calculated, best lane to drive is determined. The lane with maximum free space ahead is selected. While selecting the best lane, distance of car coming from behind is also taken into consideration to avoid another car hitting us. This is implemented in code from [line 148 to 168](https://github.com/liveusr/CarND-Path-Planning-Project/blob/master/src/main.cpp#L148). Following image shows lane change:

![alt text](writeup_data/output_lane_change.gif "output_video")

**3. Initiate lane change if required**<br>
After best lane to drive is decided, model checks if lane change is required or not. Model also checks if it is possible to change lane or not. If we are already in the process of changing lane, then this new change discarded until current lane change is completed. This is implemented in code from [line 170 to 182](https://github.com/liveusr/CarND-Path-Planning-Project/blob/master/src/main.cpp#L170). 
If current lane is one of the side lanes and target lane is another side lane, model can also transition from one side lane to another via middle lane. Following image shows switching from one side lane to another:

![alt text](writeup_data/output_multiple_lane_change.gif "output_video")


**4. Adjust target velocity**<br>
Once target lane is decided, model adjusts reference velocity. If we are driving slower than road speed, velocity is increased gradually. If slower traffic is driving ahead and lane change is not possible, model reduces our velocity and adopts speed of car driving in front. This is implemented in code from [line 184 to 195](https://github.com/liveusr/CarND-Path-Planning-Project/blob/master/src/main.cpp#L184). Following image shows adjusting of speed:

![alt text](writeup_data/output_reduce_speed.gif "output_video")

**5. Calculate anchor waypoints along the trajectory**<br>
After selecting target lane and speed, model calculates a few waypoints along the trajectory. Car's previous path's endpoints are used as reference points to create a new trajectory. This ensures smooth transition. If previous path is not available, car's current state is used as reference. This is implemented in code from [line 212 to 269](https://github.com/liveusr/CarND-Path-Planning-Project/blob/master/src/main.cpp#L212).

**6. Create spline and find out points for target trajectory**<br>
These waypoints are used as anchor points to create a spline. This spline is split in proportion to reference velocity and new coordinates are calculated. Simulator does not understand Frenet Coordinates, so these coordinates are converted to map (x,y) coordinates. These new points then fed back to simulator. This is implemented in code from [line 271 to 310](https://github.com/liveusr/CarND-Path-Planning-Project/blob/master/src/main.cpp#L271).

#### Result

After successfully implementing the path planner, car could -
<br> - complete a loop around the highway and drive more than 4.32 miles without incident.
<br> - follow speed limit of 50 MPH and reduce its speed when obstructed by traffic.
<br> - limit total acceleration to 10 m/s^2 and a jerk of 10 m/s^3.
<br> - keep safe distance from other cars on road.
<br> - stay in its lane.
<br> - change lane when required within 3 seconds.

Following GIF shows snippet from actual test run in the simulator:

![alt text](writeup_data/output_video.gif "output_video")

Complete output video can be found here: [output_video.mp4](writeup_data/output_video.mp4)

