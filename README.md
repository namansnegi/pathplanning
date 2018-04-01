# Udacity Self-Driving Car Engineer Nanodegree
# Path Planning Project

## Introduction

The goal of this project is to navigate a car around a simulated highway scenario, including traffic and given waypoint, telemetry, and sensor fusion data without violating set of motion constraints, namely maximum velocity, maximum acceleration, and maximum jerk, and also avoiding collisions with other vehicles, keeping to within a highway lane and changing lanes when doing so is necessary to maintain a speed near the posted speed limit.


## Implementation

### 1.Prediction

The sensor fusion data received from the simulator in each iteration is parsed and trajectories for each of the other cars on the road are generated. These trajectories match the duration and interval of the car's trajectories generated for each available state and are used in conjunction with a set of cost functions to determine a best trajectory for the car. 

### 2. Determine Trajectory

Using the car "planning state", sensor fusion predictions, and Vehicle class methods, an optimal trajectory is produced. 

1. Available states are updated based on the car's current position, with some extra assistance from immediate sensor fusion data 
2. Each available state is given a target Frenet state (position, velocity, and acceleration in both s and d dimensions) based on the current state and the traffic predictions. 
3. A quintic polynomial, jerk-minimizing (JMT) trajectory is produced for each available state and target.
4. Each trajectory is evaluated according to a set of cost functions, and the trajectory with the lowest cost is selected. The cost functions include:
  - Collision cost: penalizes a trajectory that collides with any predicted traffic trajectories.
  - Buffer cost: penalizes a trajectory that comes within a certain distance of another traffic vehicle trajectory.
  - In-lane buffer cost: penalizes driving in lanes with relatively nearby traffic.
  - Efficiency cost: penalizes trajectories with lower target velocity.
  - Not-middle-lane cost: penalizes driving in any lane other than the center in an effort to maximize available state options.

### 3. Proposed New Path

The new path starts with a certain number of points from the previous path, which is received from the simulator at each iteration. From there a spline is generated beginning with the last two points of the previous path that have been kept (or the current position, heading, and velocity if no current path exists), and ending with two points 30 and 60 meters ahead and in the target lane. This produces a smooth x and y trajectory. To prevent excessive acceleration and jerk, the velocity is only allowed increment or decrement by a small amount, and the corresponding next x and y points are calculated along the x and y splines created earlier. 

## Conclusion

The resulting path planner works well. It has managed to accumulate incident-free runs of over ten miles multiple times, and once navigating the track incident-free for over twenty miles. Improving the planner from this point is difficult due to the infrequency of infractions and inability to duplicate the circumstances that led up to an infraction. 

![22 miles](./screenshot.png)
