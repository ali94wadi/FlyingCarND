## Project: 3D Motion Planning
---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
These scripts contain a basic planning implementation that includes a state machine that can direct the behavior of the Drone between the states, incuding sending waypoints to the Drone. The motion planner discritizes the environment into a grid, checks for collisions with the environment, trims and prunes in between the waypoints generated between start and end points using a collinearity check, and A* is used to pan the path throughout the environment.

### Implementing Your Path Planning Algorithm

#### 1. Set your global home position

#### 2. Set your current local position

#### 3. Set grid start position from local position

#### 4. Set grid goal position from geodetic coords

#### 5. Modify A* to include diagonal motion (or replace A* altogether)

#### 6. Cull waypoints 

### Execute the flight

Attempt 1
======
https://user-images.githubusercontent.com/23568809/213751032-d6f97fcb-d379-45bf-b868-53e1d3ecde5c.mp4


Attempt 2
======
https://user-images.githubusercontent.com/23568809/213849171-83b8fbd0-773f-4d97-9553-a3c28c6e9532.mp4


  
# TODO Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.


