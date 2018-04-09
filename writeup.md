# Implementation Details

---

## The Model

This project uses the Kinematic model as described in the lectures which also tracks how errors change over time by including them in the state vector. There are 6 components that describe the state: position x, position y, heading psi, velocity v, crosstrack error cte, heading error epsi. The goal of MPC is to find the optimal values of the actuators 'throttle' and 'steering angle' to be applied to the vehicle to best follow the expected trajectory.

x 
t+1
​  =x 
t
​  +v 
t
​  cos(ψ 
t
​  )∗dt

y_{t+1} = y_t + v_t sin(\psi_t) * dty 
t+1
​  =y 
t
​  +v 
t
​  sin(ψ 
t
​  )∗dt

\psi_{t+1} = \psi_t + \frac {v_t} { L_f} \delta_t * dtψ 
t+1
​  =ψ 
t
​  + 
L 
f
​  
v 
t
​  
​  δ 
t
​  ∗dt

v_{t+1} = v_t + a_t * dtv 
t+1
​  =v 
t
​  +a 
t
​  ∗dt

cte_{t+1} = f(x_t) - y_t + (v_t sin(e\psi_t) dt)cte 
t+1
​  =f(x 
t
​  )−y 
t
​  +(v 
t
​  sin(eψ 
t
​  )dt)

e\psi_{t+1} = \psi_t - \psi{des}_t + (\frac{v_t} { L_f} \delta_t dt)eψ 
t+1
​  =ψ 
t
​  −ψdes 
t
​  +( 
L 
f
​  
v 
t
​  
​  δ 
t
​  dt)