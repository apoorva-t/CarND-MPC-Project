# Implementation Details

---

## The Model

This project uses the Kinematic model as described in the lectures which also tracks how errors change over time by including them in the state vector. There are 6 components that describe the state: position x, position y, heading psi, velocity v, crosstrack error cte, heading error epsi. The goal of MPC is to find the optimal values of the actuators 'throttle' and 'steering angle' to be applied to the vehicle to best follow the expected trajectory.

![alt text](./state_eqs)

## Timestep Length and Elapsed duration

When running MPC without any latency, values of N=10 and dt=0.1 were found to work best. Keeping the value of dt as low as possible makes sure we avoid discretization error. Increasing the value of N will lead to an increased computation time as the number of variables required to be optimized by the optimizer will increase.

However, on introducing a latency of 0.1s in applying the actuators, a dt of 0.1s lead to oscillations. This may be because the actuations get computed every dt seconds, but are getting applied with a delay of 0.1s, not allowing them time to settle in. As suggested on the SDCND discussion forum changing the dt to 0.2 resolves this issue and resulted in the car following the trajectory smoothly.

## Polynomial fitting and MPC preprocessing

The waypoints sent from the simulator are transformed to the car's co-ordinate system. Then a 3rd degree polynomial is fit to these transformed waypoints. Since the processing is now being done in the car's co-oridnate system, the vehicle state (px,py) co-ordinates are set to (0,0) (car's origin). And the heading angle 'psi' is also set to zero as the transformed x axis is in the direction of the car heading.

## Model Predictive Control with Latency

To handle the 100ms latency in the actuations to take effect, my initial thought was to use the actuator outputs from a later time step, say N steps later to pass back to the simulator. However, this did not seem to work as well. Then based on some recommendations read on the Slack group, I chose to go with the approach of using the vehicle kinematic model to predict the state after 100ms, and pass that as the initial state to the solver. Thus the solver computes the actuator outputs for a state 100ms after the current state. Also the penalty to cost because of 'cte', heading error and 'delta' i.e. steering were increased to make the car drive smoother.