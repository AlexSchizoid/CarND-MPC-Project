
## Rubric
### The model
The model used in this project is called the Kinematic Model. It can be described by a state vector, vehicle actuators and a set of simple equations. The Kinematic model ignores some elements such as mass and gravity. The state vector contains parameters such as:
* vehicle x,y coordinates
* psi - orientaiton angle
* velocity
* cross track error - cte
* psi error - epsi

The outputs of the model are the vehicle actuators: steering angle of the car(delta) and acceleration. The model uses the information vector from the previous timestamp  and the model equations to predict the state of the car for next few timestamps. This calculation is repeated after every new timestamp. The equtaions used are the following:

* x_t+1 = x_t + v_t * cos(psi_t) * dt
* y_t+1 = y_t + v_t * sin(psi_t) * dt
* psi_t+1 = psi_t + v_t/Lf * delta * dt
* v_t+1 = v_t + a_t * dt
* cte_t+1 = cte_t + v_t * sin(epsi_t) * dt
* epsi_t+1 = epsi_t + v_t/Lf * delta_t * dt

### Timestep Length and Elapsed Duration
The MPC uses the state vector to predict a set of N points(timestep length) with a duration of dt, also called the elapsed duration, between them. This points declare the trajectory the vehichle should take, which should minimize the error. The duration taken into account by the MPC is N * dt. The values i've chosen is N = 10 and dt = 0.1 which means MPC predicts a 1s time frame. I've also tried multiple values for N, going as far as N = 15 before the car was no longer driving reliably. The values that work are dependent on the hardware used to run the mpc - the higher the value the more work the IPOPT solver must do in the same configured timeframe(max 0.5s in my case). I've chosen the value 10 and 0.1 as it allows the car to drive reliably - also it means you can replicate the simulation on a less powerful PC.

### Polynomial Fitting and MPC Preprocessing and Latency
Before giving the state vector to the MPC we preprocess it a bit in order to more easily fit a third order polynomial. The waypoints are transformed from map space to vehicle space - the car has 0,0 coordinates and 0 psi. The polynomial is evaluated for x = 0 which gives us the cte. 

Latency is also a factor to take into account. In a real car it takes time until an actuation is actually carried out, so in order to simulate this, the main loop sleeps for 100ms. This means that using the previous timestamp data is no longer enough. In order to solve this problem a straight-forward solution is to estimate the next timestamp data(next 100ms) - by using the kinematic equations - and deploy that set of values as input for the solver. The solver will output values that deal with the latency issue. 
