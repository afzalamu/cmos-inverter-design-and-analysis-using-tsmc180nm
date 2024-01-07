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
and from this curve, we can obtain the ```gm``` transconductance curve for NMOS, by editing the command in the waveform viewer in LT Spice as ```d(Id(M2))```.The deriv() function takes the derivative with respect to the independent variable present at the current simulation. From the definition of Gm we are aware that it is dIds/dVds.
![gm_nmos](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/5ef81e46-10cb-4ccb-83de-87f44f617a13)
and now, we will calculate the Threshold voltage for NMOS ```vtn```
