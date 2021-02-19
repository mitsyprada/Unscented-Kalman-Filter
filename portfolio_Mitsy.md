# Portfolio DFaAM

This portfolio aims to address all the assessment aspects of the rubric, one by one and in order. Many of the assessment aspects are justified by work done in GitHub repositories or in one of the appendixes. Either way, this is indicated in each assessment aspect. If any error occurs trying to access to these links please contact me.

## Assessment aspects

1. *The student can create data processing software which combines multi-sensor data into correct interpretations.*

   Traffic lights project, introduction project at the beginning of the master. We combine infrared data (various sensors) to control a four way intersection, the system also receives a signal every time a siren sound is detected (with a microphone), and then the traffic lights turn . This is done by using a microphone as one of the inputs (we did some analysis on the frequencies). The system uses 2 Arduinos, one for controlling the traffic lights and another where the siren detection is implemented.

   Arduino code to control the traffic lights according to data coming from infrared sensors and microphone:

   https://hanzenl-my.sharepoint.com/:u:/g/personal/m_prada_herreno_st_hanze_nl/Ebbm6-HtiE5HrZrgYhEeRrwB3w3BWXW9BP7efL8vMCDDBg?e=T84mM8

   Code that detects a the siren:

   https://hanzenl-my.sharepoint.com/:u:/g/personal/m_prada_herreno_st_hanze_nl/EWY_CmMHWutHjayCJVpzxowBwStUScDGZ9fKbtvf4isdmQ?e=AQRM8e

   Video siren detection demonstration:

   https://hanzenl-my.sharepoint.com/:v:/g/personal/m_prada_herreno_st_hanze_nl/Ed8nFRq8n-hImuJrR0h1sxsBndO0wjOh8ZcZo2STDtZxdw?e=FfiaA9

   Video no infrared sensors demonstration:

   https://hanzenl-my.sharepoint.com/:v:/g/personal/m_prada_herreno_st_hanze_nl/EQ_5gIMga9NOmvGGiuz_pdUBp3PS0pHCwqYWViht5bPMmg?e=MBJd9O

2. *The student can analyze system requirements to identify applicable sensor fusion architectures.*  

   See Appendix 1: JDL essay

3. *The student can adapt sensor system design to create correct temporal and spatial alignment and normalization of multi-sensor data..*

   Tropomi repository includes 3 types of data representation (under 'data visualization' in the readme): basemap, interactive plots and a statistical representation. The ch4 concentrations are extracted from the data files. For the basemap we used a matplotlib tool, for the interactive plots we used Folium and for the statistical representation we just used pyplot.

4. *The student can adapt sensor system design to create correct temporal and spatial alignment and normalization of multi-sensor data.* 

   In "tropomi project-CH4 average.ipynb" notebook previously selected (manually) Sentinel5P data is used for analyzing methane levels in China. 

- Temporal alignment: This data is processed to select data from a certain period of time (one month), and show the methane level values (average, maximum and second percentage) of each day. The data shown in graphs is from the indicated dated in the x axis.

- Spatial alignment: The program takes a rectangle, defined by min_Latitude, max_Latitude, min_Longitude, max_Longitude; and it selects only the data within the indicated coordinates.

- Normalization: Normalization can have different meanings according to The Oxford Dictionary of Statistical Terms.  A usage in statistics is the creation of shifted and scaled versions of statistics, where the intention is that these values allow the comparison of corresponding normalized values for different datasets. In this way, we normalized the methane concentrations by eliminating the empty values (fill values) and not taking these into account for plotting the data. This can be seen looking at the missing dates of the plots. This is the proposal for dealing with normalization

5. *The student can design interfaces for sensor system management.* 

   We designed interfaces in the tropomi project that allow to see CH4 concentration values over a certain area and during a certain time. See Appendix 1: JDL essay / Tropomi and JDL level 5

5. *The student can create a sensor system which combines multi-sensor data into correct interpretations.* 

   This is done in Tropomi project, providing information about CH4 concentrations: average, maximum and second percentage, in "tropomi project-CH4 average.ipynb" notebook.

6. *The student can apply mathematical data fusion models, for example: covariance intersection, unscented information, error propagation techniques.*

   Unscented information is used in the UKF assignment in this repository. See Appendix 3: Unscented Kalman Filter.

   Monte Carlo experiments is an error propagation technique. 100 Monte Carlo experiments are done in part a) of one of the assignments of the adaptive filtering course. You can see this here: [https://github.com/mitsyprada/AdaptiveFiltering/blob/master/AF4/Chapter%207%2C%20problem%2010.ipynb](https://github.com/mitsyprada/AdaptiveFiltering/blob/master/AF4/Chapter 7%2C problem 10.ipynb)

7. *The student can explain where to use device virtualization.*

   See Appendix 2: Device virtualization paragraph





## Appendixes

### Appendix 1: JDL essay

Papers of choice: 

- E. Blasch, "Level 5 (User Refinement) issues supporting Information Fusion Management," 2006 9th International Conference on Information Fusion, Florence, 2006, pp. 1-8, doi: 10.1109/ICIF.2006.301581.
- D. L. Hall, S. A. H. McMullen and C. M. Hall, "New perspectives on level-5 information fusion: The impact of advances in information technology and user behavior," 2015 IEEE International Conference on Multisensor Fusion and Integration for Intelligent Systems (MFI), San Diego, CA, 2015, pp. 214-219, doi: 10.1109/MFI.2015.7295811.

These two papers focus on JDL level 5 in a deeper level compared to the other papers, without repeating authors, making it easier to involve this JDL level 5 ideas in the interface design. 

Standard of choice:

- SOSA

Sensor Open System Architecture. It describes an Open System Architecture via modular decomposition and associated interfaces between the modules.  The part of "associated interfaces" is the most important of SOSA to take into account for the interface design of tropomi data. Reading this standard gives a better understanding of what part of an OSA the interface belongs to, and how this interface is supposed to be.

#### Tropomi and SOSA

The SOSA consortium develops an architecture which is "The fundamental organization of a system embodied in its components (modules), their relationships (interfaces) to each other and to the environment, and the principles guiding its design and evolution". In our case we want to focus on the interface part, therefore our interface is part of an Open System Architecture (satellite Sentinel 5P).

Our interface is logical, and there is an information exchange between the satellite and our interface. Our interface uses the data to show methane levels and detect methane leakages . 

SOSA describes the gray box as follows: 

Gray Box Concept: The OSA defines what the box does and the interfaces between boxes, but NOT how it does it (the IP is inside the box).  In this way we assume that our project is a part of SOSA that, in case is useful by other organizations, it needs to be connected with the other parts of the OSA based system.

According to SOSA, interface boundaries need to be well defined and well understood. Tropomi project aims to show methane concentrations and methane leakages for the area of China. According to SOSA: "Interfaces are very well defined and yet contain a high degree of flexibility". The interface is flexible, because we can choose to download data from different areas of the world and different dates, and yet used the same way of processing it to show methane concentrations and detect methane leakages. This could be automated, but this is part of a bigger project for which we would need more time of development.

#### Tropomi and JDL level 5

The delivery of fusion systems require require presenting fusion results for knowledge representation (fusion estimation) and knowledge reasoning (control management). This is why we need an interface that allow us to see the CH<sub>4</sub> concentrations, and detect leakages. JDL level 5 is focused on user refinement, it's an element of knowledge management. In this case everyone with internet access and a computer can have access to sentinel 5P data, but this data needs an interface to be displayed and support cognitive decision making to the interested parts. "When a user interacts with an IF system, it is important to support knowledge reasoning", this means explain what the user sees in each graph or CH<sub>4</sub> concentrations map, and make clear what everything means. We need to:

(1) incorporate the user in the design process

​    We are also the users, but we incorporated 

(2) gathering user needs

(3) providing the user with available control actions.

### Appendix 2: Device virtualization paragraph

**Find a definition:**

From: Developing Applications with Oracle Internet of Things Cloud Service. (2020). Retrieved 21 June 2020, from https://docs.oracle.com/en/cloud/paas/iot-cloud/iotgs/device-virtualization.html

Virtualization is the act of creating a virtual (rather than physical) version of a computer application, storage device, or a computer network resource. Virtualization makes the logical server, storage, and network independent of the deployed physical infrastructure resources.

**Explain where to use device virtualization in Smart Systems:**

From: Dileep K.P, Devesh G, A. Raghavendra Rao, Suman M and S. V. Srikanth, "Verification of Linux device drivers using device virtualization," *2015 2nd International Conference on Computing for Sustainable Global Development (INDIACom)*, New Delhi, 2015, pp. 694-698.

Board Support Packages are the essential code code for a given computer hardware device that will make that device work with the computer's OS. In order to quickly test developed BSPs (Board Support Packages) and device drivers without the real hardware and with continuous change in I/O (input/output) devices for various applications, there is a need for platform and device virtualization. This is just one of the uses for device virtualization of I/O devices.

**Example in Smart Systems project**

Virtualization of storage device for the humidity, temperature and airflow values that come from the sensors of the simulation, saved with their time stamps. This can be provided by a cloud service. 

### Appendix 3: Unscented Kalman Filter

Kalman Filter is used for prediction of measurable variables that are part of the state of an object. A Kalman filter is an iterative mathematical process that uses a set of equations and consecutive data inputs to quickly estimate the a true value (measurable state) of the object being measured, when the measured values contain unpredicted or random error, uncertainty or variation.

However, KF is presents problems for non linear problems, because its predictions are made using linear algebra. For predicting a non linear behavior (that follows a non linear function) of an object we can use the Unscented Kalman Filter. 

The UKF can use many different algorithms to generate sigma points (in my case I used MerweScaledSigmaPoints) and weights in a non linear way (which would give big errors for a non linear system like tracking a pendulum). Through the Unscented Transform the sigma points pass through a non linear function, obtaining a transformed set of points, for which we compute the covariance and mean that become the new estimate.

In the UKF repository I apply the UKF algorithm to track a pendulum. This is compared to the naive g-h filter already implemented. The purple circle is the naive g-h filter and the green circle is the implementation of the UKF. The green circle follows better the yellow one (the actual movement of the pendulum), and this can be seen running the code. The error estimation for kf and ukf are printed when we stop the loop.