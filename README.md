# Unscented Kalman Filter

The unscented Kalman filter (UKF) is a data fusion method to fuse a non-linear model with measurement data. Both the dynamical model and the coordinates transforms involved can become nonlinear . 

I created a simulation of a pendulum which produces simulated measurement data. It has a naive g-h filter implemented, and I would like to have it compared with the UKF. Unfortunately I got stuck at understanding how to pick initial values for the UKF . 

So with your help I would like to amend the program and implement a UKF demonstrator.

```python
'''
Simulating the equation $mL^2\ddot{\theta} + b \dot{\theta} +mgL sin(\theta) = 0$
'''
import numpy as np
import cv2
import time

# visulaization parameters
height = 600
width = 600

# simulation 
center = None

# Pendulum parameters and variables

#    initial conditions
theta = np.pi/2.*.5
omega = 0

#    parametrs
g = 0.98 #9.8 # m/s^2
L = .3 # m
m = 0.05 # kg
b = 0 

# Numerical integration parameters
framerate = 60.0 # in frames per second
dt = 1.0/framerate # Set dt to match the framerate of the webcam or animation
t = time.clock()

# Drawing parametres
thickness = 3

# Noise parameters 
Sigma = 30*np.array([[1, 0],[0,1]])

# Kalman inferred state variables (g-h model like right now)
theta_kf = theta
omega_kf = omega
theta_kf_old = theta_kf
# Keep looping

# Create background image
frame = np.zeros((height,width,3), np.uint8)

center_old = (300, 300)
center_noisy_old = (300, 300)
center_kf_old = (300, 300)


L_kf= 200
# Create background image
frame = np.zeros((height,width,3), np.uint8)
	
cv2.circle(frame, (300, 300), 10, (0, 255, 255), -1)
	
	
while True:
	cv2.circle(frame, (300, 300), 10, (0, 255, 255), -1)
	# == Simulation model ==
	
	# Update state 
	theta = theta + dt*omega
	omega = omega - dt*g/L*np.sin(theta) - dt*b/(m*L*L)*omega 
	
	# Map the state to a nearby pixel location
	center = np.array((int(300+ 200*np.sin(theta)) ,int(300 + 200*np.cos( theta))) ) 
	# center = (int(300+ 100*theta) ,int(300 + 25*omega)) 
	center_noisy = tuple(center+np.matmul(Sigma,np.random.randn(2)).astype(int))
	
	# Draw the pendulum
	#cv2.line(frame, (300,300), tuple(center), (0, 0, 255), thickness)
	cv2.circle(frame, tuple(center_old), 10, (0, 0, 0), -1)
	cv2.circle(frame, center_noisy_old, 10, (0, 0, 0), -1)
	
	
	cv2.circle(frame, tuple(center), 10, (0, 255, 255), -1)
	cv2.circle(frame, center_noisy, 10, (0, 0, 255), -1)

	center_old = center
	center_noisy_old = center_noisy
	
	
	# == Kalman model ==
	# Prediction
	theta_kf +=   dt*omega_kf 
	omega_kf += - dt*g/L*np.sin(theta_kf) - dt*b/(m*L*L)*omega_kf 
	
	# expected observation
	center_kf = np.array((int(300+ L_kf*np.sin(theta_kf)) ,int(300 + L_kf*np.cos( theta_kf))) ) 
	
	# Observation Update 
	observation = center_noisy 
	
	print(observation)
	theta_observed = np.arctan( (observation[0]-300)/(observation[1]-300))
	L_observed= np.sqrt(np.power(observation[0]-300,2)+np.power(observation[1]-300,2))
	theta_gain = 0.2
	omega_gain = 0
	L_gain = 0.02
	theta_kf += theta_gain*(theta_observed-theta_kf)
	omega_kf += omega_gain*((theta_kf-theta_kf_old)/dt -omega_kf)
	L_kf += L_gain *(L_observed-L_kf)
	theta_kf_old = theta_kf 
	center_kf = np.array((int(300+ L_kf*np.sin(theta_kf)) ,int(300 + L_kf*np.cos( theta_kf))) ) 
	
	# Map the state to a nearby pixel location
	
	cv2.circle(frame, tuple(center_kf_old), 10, (0, 0, 0), -1)
	center_kf_old = center_kf
	cv2.circle(frame, tuple(center_kf), 10, (255, 0, 255), -1)
	
	# show the frame to our screen
	cv2.imshow("Frame", frame)
	key = cv2.waitKey(int(dt*400)) & 0xFF

	# if the 'q' key is pressed, stop the loop
	if key == ord("q"):
		break
	
	
	# Wait with calculating next animation step to match the intended framerate
	t_ready = time.clock()
	d_t_animation = t + dt -  t_ready
	t += dt
	if  d_t_animation > 0:
		time.sleep(d_t_animation)
		
		
	
		
		

# close all windows
cv2.destroyAllWindows()
```


## Literature:

Some of the links below require you to be logged in to Hanze.nl

### Method Papers

Necessary:

S.J. Julier and J.K. Uhlmann, *Unscented Filtering and Nonlinear Estimation*,  Proceedings of the IEEE  vol. 92 no. 3 (March 2004): 401-422,   [10.1109/JPROC.2003.823141](https://hanze.on.worldcat.org/oclc/4655140781)

E.A. Wan and R. van der Merwe, *The unscented Kalman filter for nonlinear estimation*
Proceedings of the IEEE 2000 Adaptive Systems for Signal Processing, Communications, and Control Symposium, Cat. No.00EX373,  [IEE Explore](https://ieeexplore.ieee.org/xpl/conhome/7076/proceeding), [Author version](https://www.seas.harvard.edu/courses/cs281/papers/unscented.pdf).

Roger Labbe,  Kalman and Bayesian filters in Python, [Book and Code on Github](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python), Chapters 1, 8 and 10.

Optional:

E.A. Wan,  R. van der Merwe and A.T. Nelson, *Dual Estimation and the Unscented Transformation*, [NIPS](https://papers.nips.cc/paper/1681-dual-estimation-and-the-unscented-transformation.pdf)

### Application Papers

J. H. Gove1and D. Y. Hollinger, JOURNAL OF GEOPHYSICAL RESEARCH, VOL. 111, D08S07, doi:10.1029/2005JD006021, 2006 [Wiley](https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2005JD006021)



