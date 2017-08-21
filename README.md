# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---
This project implements a Model Predictive Controller for Udacity’s self driving car simulator.

## The Model

I implemented a kinematic model as described in class.

The model is formed by 6 variables: position (x and y), speed,  orientation, cross track error, and orientation error.

The car has two actuators, it can accelerate/brake, and steer.

Update equations for this model are:

x1 = (x0 + v0 * cos(psi0) * dt)

y1 = (y0 + v0 * sin(psi0) * dt)

psi1 = (psi0 + v0 / Lf * delta0 * dt)

v1 = (v0 + a0 * dt)

cte1 = ((f0 - y0) + (v0 * sin(epsi0) * dt))

epsi1 = ((psi0 - psides0) + v0 * delta0 / Lf * dt)

In those equations v is speed, x and y are positional coordinates, psi is the orientation, delta is the steering angle, Lf the distance between the front of the vehicle and it center of gravity, a is the accelerator pedal actuation, psides is the desired orientation, cte is the cross track error, epsi is the orientation error and dt is the time step.

## Timestep Length and Elapsed Duration (N & dt)

I played around with different values of N and dt. First I tried to make the car stable. Once I got it working I started to increase speed. I combined playing with this value and with the weights I used on each cost of the optimizer. In the end, and considering that with this kind of model it is not very useful to go very far into the future I settle for having a step of 0,075 seconds and using 10 steps. This results in a modeled trajectory up to 0,75 second into the future. This configuration also has to do with how I deal with latency. 

## Latency

The way I used to deal with latency was not using the first control command that I got from the optimizer. I figure that if I was using a time step of 0,075 seconds I could use the command that was going to be useful 0,15 seconds from now. This helps dealing with a latency of 0,1 seconds. This is not necessarily a robust solutions since it is based on an assumed latency and an imperfect prediction. A more dynamic adjustment would be better, for example assuming we are compliying with our predicted trajectory and use its actuation commands until we receive new feedback.