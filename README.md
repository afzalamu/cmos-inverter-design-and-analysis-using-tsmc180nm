# CMOS Inverter Design and Analysis Using TSMC180nm

## Description

The motive of this project is to familiarize users with the LTspice tool and experiment with the working of a CMOS inverter, exploring various parameters associated with it. The project utilizes the TSMC180nm model file and begins by analyzing MOSFET models. The goal is to understand different plots and extract important parameters related to MOSFETs. Subsequently, the project progresses to the analysis of a CMOS inverter, where the aim is to obtain relevant plots and extract key parameters associated with its performance.

## 1. MOSFET Models Analysis
### NMOS Analysis
In this section, I start with our analysis of MOSFET models present in tsmc180nm. Here, ```Vdd=1.8V``` and ```L=180nm``` as in the case of digital circuits Length can be taken as a minimum, and width is generally taken 1.5 times of length.
So, first I am plotting ```Id vs vgs```  curve, and here is the LT spice setup for that.

![setup_idvsvgs_nmos](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/e15fd56e-85a8-4bfa-9fa7-2517f9f95df7)

The curve is plotted between ```Id vs vgs``` by sweeping vgs from ```0 to 1.8V```for different values of vds, for this use the commands shown in the schematic.

![idvsvgs_curve_nmos](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/1fac79fd-5481-4378-8308-732151c8d154)

This shows us that the threshold value is between 600mV to 700mV but we are going to calculate it using setup also.
Also from this curve, we can obtain the ```gm``` transconductance curve for NMOS, by editing the command in the waveform viewer in LT Spice as ```d(Id(M2))```.The deriv() function takes the derivative with respect to the independent variable present at the current simulation. From the definition of Gm we are aware that it is dId/dVgs.

![gm_nmos](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/5ef81e46-10cb-4ccb-83de-87f44f617a13)

Now, I am plotting ```Id vs vds``` curve, and here is the LT spice setup for that.

![image](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/bea0ebbc-16c1-40e3-a740-f62e28649df4)

The curve is plotted between ```Id vs vds``` by sweeping vds from ```0 to 1.8V```for different values of vgs, for this use the commands shown in the schematic.
and from this curve, we can obtain the ```ro_nmos```  for NMOS, by editing the command in the waveform viewer in LT Spice as ```d(Id(M2))```.The deriv() function takes the derivative with respect to the independent variable present at the current simulation. From the definition of ```ro``` we are aware that it is dId/dVds.
and now, we will calculate the Threshold voltage for NMOS ```vtn``` here is the LT Spice setup for that.

![vtn_setup](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/095cf988-ed45-4314-a564-c809be1ef59a)

Here, we have assumed a reasonable value of vgs ( greater than vtn) and using the spice error log we will find ```vtn``` and here we are taking ```width = 0.36u```.
Now, you can view the spice error log file in LT Spice by using ``` ctrl + L``` and can see ```vtn ```value.

![vtn_value](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/ec25154c-1e7d-4d04-b4e6-b9950b380306)

Hence, we now have all the important values we need. The same can be done for a PMOS. 

![pmos setup](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/0e1cb595-7d56-4d83-bcea-4694419d7780)

The motive is the same, but especially to extract the value of the Aspect ratio for which the current is the same in both NMOS and PMOS.
I have done some experimentation and found that at W/L of PMOS = 4 * (Aspect ratio of NMOS), the current value is pretty close.
So, we found the NMOS had a current of 9.1 microamps while PMOS had a current of 9.5 microamps (both at |Vgs| = 0.65V). (NOT BAD!!)




