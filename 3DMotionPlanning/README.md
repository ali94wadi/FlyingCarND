## Project: 3D Motion Planning
---


# Required Steps for a Passing Submission:
- [x] Load the 2.5D map in the colliders.csv file describing the environment.
- [x] Discretize the environment into a grid or graph representation.
- [x] Define the start and goal locations.
- [x] Perform a search using A* or other search algorithm.
- [x] Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
- [x] Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
- [x] Write it up.

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
These scripts contain a basic planning implementation that includes a state machine that can direct the behavior of the Drone between the states, incuding sending waypoints to the Drone. The motion planner `motion_planning.py` discritizes the environment into a grid, checks for collisions with the environment, trims and prunes in between the waypoints generated between start and end points using a collinearity check, and A* is used to pan the path throughout the environment.

All the functunal backbones of the planner are available in `planning_utils.py`.

### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
`read_pos(.)` is written to extract the lat, lon from the `colliders.csv` file and the `self.set_home_position(.)` method is used to update the home position of the Drone

```python
def read_pos(filename):
    """read the first line of a csv file.
    Create a list of the float numbers in the string,
    Discard the rest of the line.
    Example line:
    lat0 37.792480, lon0 -122.397450
    Example output:
    37.792480, -122.397450"""
    with open(filename) as f:
        line = f.readline()
        data = line.split()
        lat = data[1]
        lon = data[3]
        return lat, lon
```

#### 2. Set your current local position
The provided `global_to_local(.)` function is used to do this

#### 3. Set grid start position from local position
A grid is defined using the provided `create_grid(.)` function, and the `grid_start` variable is initilized to be the current local position using the provided function
```python
grid_start = (int(north - north_offset), int(east - east_offset))
```

#### 4. Set grid goal position from geodetic coords
The grid goal is set to some latitude / longitude position at random or by user input, and it then converted to local position using the provided function like in point 3 above
```python
global_goal = self.global_home + (5*np.random.normal(0, 0.0001), 5*np.random.normal(0, 0.0001), -5)
```       

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
To do this, the following new actions with associated costs were programmed into the `Action(Enum)` class
```python
SOUTH_EAST = (1, 1, 1.414)
SOUTH_WEST = (1, -1, 1.414)
NORTH_EAST = (-1, 1, 1.414)
NORTH_WEST = (-1, -1, 1.414)
```
and `valid_actions(.)` method was also updated with the following actions
```python
if x + 1 > n or y + 1 > m or grid[x + 1, y + 1] == 1:
    valid_actions.remove(Action.SOUTH_EAST)
if x - 1 < 0 or y + 1 > m or grid[x - 1, y + 1] == 1:
    valid_actions.remove(Action.NORTH_EAST)
if x + 1 > n or y - 1 < 0 or grid[x + 1, y - 1] == 1:
    valid_actions.remove(Action.SOUTH_WEST)
if x - 1 < 0 or y - 1 < 0 or grid[x - 1, y - 1] == 1:
    valid_actions.remove(Action.NORTH_WEST)
```

#### 6. Cull waypoints 
To do this, following the learning exercises, the following three functions are used to check for linearity in the waypoints, and prune the path such that it is shortened
```python
def collinearity_check(p1, p2, p3, epsilon=1e-6):   
    m = np.concatenate((p1, p2, p3), 0)
    det = np.linalg.det(m)
    return abs(det) < epsilon
```
```python
def point(p):
    return np.array([p[0], p[1], 1.]).reshape(1, -1)
```
```python
def prune_path(path):
    pruned_path = [p for p in path]
    # TODO: prune the path!
    
    i = 0
    while i < len(pruned_path) - 2:
        p1 = point(pruned_path[i])
        p2 = point(pruned_path[i+1])
        p3 = point(pruned_path[i+2])
        
        # If the 3 points are in a line remove
        # the 2nd point.
        # The 3rd point now becomes and 2nd point
        # and the check is redone with a new third point
        # on the next iteration.
        if collinearity_check(p1, p2, p3):
            # Something subtle here but we can mutate
            # `pruned_path` freely because the length
            # of the list is check on every iteration.
            pruned_path.remove(pruned_path[i+1])
        else:
            i += 1
    return pruned_path
```
### Execute the flight

Attempt 1
======
https://user-images.githubusercontent.com/23568809/213751032-d6f97fcb-d379-45bf-b868-53e1d3ecde5c.mp4


Attempt 2
======
https://user-images.githubusercontent.com/23568809/213849171-83b8fbd0-773f-4d97-9553-a3c28c6e9532.mp4


  
# TODO Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.


