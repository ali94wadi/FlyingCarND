## Project: Building a Controller
---

### Writeup / README
A few scenario test flights in simulation were used to tune the parameters of the multi-dof cascaded PID controllers used to guide the quadcopter through the trajectory following task in 3D

1. "Intro_1" 
 
 Here the ```mass``` estimate of the quadcopter is estimated by tuning and looking at the behavior of the copter when tasked to hold its position.

2. "Body rate and roll/pitch control" 

Here the following functions are implemented to stabilize the rotational motion of the quad and execute roll / pitch control.

  - ```GenerateMotorCommands()```

  - ```BodyRateControl()```

  - ```RollPitchControl()```

3. "Position/velocity and yaw angle control" 

Here the following functions are implemented to perform XY plane position control, altitude control, and yaw position control of the quad.

- ```LateralPositionControl()```

- ```AltitudeControl()```
  
- ```YawControl()```

4. "Non-idealities and robustness" 

```AltitudeControl()``` is modified here to add an integral component to the PD controller to make it PID to circumvent from errors arising from mismatch in the model used to compute motor thrusts.

5. "Tracking trajectories" 

This is a final test of the developed controllers and the corresponding gains to perform trajectory tracking of two physically different quadcopters. 

### Demo of all required scenarios

https://user-images.githubusercontent.com/23568809/214079373-423575a8-9a79-459d-a880-783f70508a3f.mov

