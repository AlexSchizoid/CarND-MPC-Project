
## Rubric
### The model
The model used in this project is called the Kinematic Model. It can be described by a state vector, vehicle actuators and a set of simple equations. The Kinematic model ignores some elements such as mass and gravity. The state vector contains parameters such as:
* vehicle x,y coordinates
* psi - orientaiton angle
* velocity
* cross track error
* psi error

The outputs of the model are the vehicle actuators: steering angle of the car(delta) and acceleration. The model uses the information vector from the previous timestamp  and the model equations to predict the state of the car for next few timestamps. This calculation is repeated after every new timestamp. The equtaions used are the following:
x_t+1 = x_t + v_t * cos(psi_t) * dt
y_t+1 = y_t + v_t * sin(psi_t) * dt
psi_t+1 = psi_t + v_t/Lf * delta * dt
v_t+1 = v_t + a_t * dt
cte_t+1 = cte_t + v_t * sin(epsi_t) * dt
epsi_t+1 = epsi_t + v_t/Lf * delta_t * dt
