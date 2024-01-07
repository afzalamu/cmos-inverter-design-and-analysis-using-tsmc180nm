# CMOS Inverter Design and Analysis Using TSMC180nm

## Description

The motive of this project is to familiarize users with the LTspice tool and experiment with the working of a CMOS inverter, exploring various parameters associated with it. The project utilizes the TSMC180nm model file and begins by analyzing MOSFET models. The goal is to understand different plots and extract important parameters related to MOSFETs. Subsequently, the project progresses to the analysis of a CMOS inverter, where the aim is to obtain relevant plots and extract key parameters associated with its performance.

## 1. MOSFET Models Analysis
### 1.1 NMOS Analysis
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

### 1.2 STRONG O AND WEAK 1
What does the above mean? Look at the schematic and waveforms below,
![strong0weak1_nmos_setup](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/c3aa5669-0c4c-4dd6-bf9d-c8aae26a280c)
![strong0weak1_nmos](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/d78b7ee1-d6b3-4ff3-a2ce-860181054bd5)

Here we can see that when a square wave is applied to the input of NMOS when it is logic LOW(0V), the output goes to HIGH(1.8V). But when the input is logic HIGH(1.8V), the output goes to a value that is much larger than 0V. This is because when Vgs is 1.8V, the NMOS is in the linear region. This is where the MOSFET acts as a voltage-controlled resistor. At this point, the output is connected to a Voltage Divider Configuration. That is the output takes the value which is defined by the voltage across the resistance of the MOSFET. Hence, NMOS can transmit STRONG 0, but not STRONG 1.

### 1.3 STRONG 1 AND WEAK 0
here also let us look at some schematic and waveforms in LT Spice

![strong1weak0pmos_setup](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/4299501d-37f4-4f5b-bdbf-e80076e037db)
![strong1weak0pmos](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/480f6815-2c84-4bed-9871-4ea273557479)
Therefore, both NMOS and PMOS alone do not serve as effective inverters. Various configurations were considered, but ultimately, one emerged as the most popular circuit design using MOSFETs, known as a CMOS configuration.


## 2. CMOS INVERTER
### 2.1. Why Choose CMOS Circuits

In the preceding section, we made an interesting observation: neither NMOS nor PMOS alone can effectively generate both HIGH and LOW values. However, they exhibit a complementary relationship. This insight led to the concept of combining them. Leveraging PMOS as a Strong 1, we position it between VDD and Vout, while NMOS, functioning as a STRONG 0, is situated between Vout and GND. By doing so, each can serve as a load for the other transistor, ensuring they are never ON simultaneously (or are they?). This arrangement is known as the Complementary Metal Oxide Semiconductor (CMOS) Configuration, representing the simplest circuit called the CMOS Inverter.

![cmosinv](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/1ef54685-d4fb-459b-9eba-a0e2c050ee75)


CMOS Circuits typically involve a network divided into two segments: the upper section is referred to as the pull-up network and the lower half as the pull-down network. The former incorporates P-channel MOSFETs, while the latter employs N-channel MOSFETs. The rationale is straightforward: as one transistor turns on, the other turns off, eliminating the issue of a resistive path to the ground and minimizing voltage division (at least not significantly). This arrangement allows for easily achieving both Strong High and Strong Low outputs from the same network. The pull-up provides a low-resistance path to VDD, while the pull-down offers a low-resistance path to GND.

<img width="1046" alt="punand pdn" src="https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/b11aa9f9-e173-42d9-a800-7e4a14b92509">

### 2.2. CMOS Inverter Design and Analysis using LT Spice
Before moving further, First, let us understand What is an Inverter?
So, In layman's terms, We can say that that inverter is the circuit that performs the NOT function or we can say that inverts and it is characterized practically by various parameters such as Noise Margin and speed.
Now, here I have designed an inverter in LT Spice, as  we had already calculated the width of PMOS as 4 times the width of NMOS using that I have taken ```W_NMOS=0.36u``` and ```W_PMOS=1.44u```.
Here is its schematic in LT Spice.

![cmosinv_sch](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/3b80a74b-e027-4be5-bfc2-16a94c7ccc16)


#### 2.2.1 DC Analysis and Important Design Parameters
We are going to plot  the Voltage Transfer Characteristics (VTC) curve for the inverter using DC analysis. 
Here we sweep the ``` Vin``` from ``` 0V to 1.8V ``` and plot the ``` vin``` and ```vout```.
using the following command ```.dc vin 0 1.8 1m```
Here is the resulting waveform:

![vtc_inv](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/37db0840-cb28-436b-bb96-fb409eee6717)

Here we can see from the curve that ```vin=vout``` at ```0.903V``` which is approximately equal to ```vdd/2``` later we will see that it is termed as the switching threshold of an inverter.
Now let us understand, What other parameters can be calculated using this curve:

![vtc_book](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/effafbef-38d0-4426-8bda-fec545308698)

we can see from the above picture that the inverter has five regions of operation.
Also, refer to the below picture:

![nm_inv](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/62b7b7fb-4de5-45ad-862f-64d480e98460)


Let us understand the important parameters of this device that are based on its VTC curve.

VOH - Maximum output voltage when it is logic '1'.
VOL - Minimum output voltage when it is logic '0'.
VIH - Maximum input voltage that can be interpreted as logic '0'.
VIL - Minimum input voltage that can be interpreted as logic '1'.
Vth - Inverter Threshold voltage (Switching Threshold which we have already calculated to be ```0.903V``` using VTC)
The above five are critical for an Inverter and can be seen on the VTC curve of an inverter. One thing to point out now would be,

Vth should be at a value of VDD/2 for maximum noise margins

Now, using the property when ```d(vout)/d(vin) = -1``` we can calculate all these parameters by plotting the ```vout``` and then applying ```d( )``` on it and using the marker for measuring in LT Spice.

![vil_and_vih](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/87d0bdcf-85dc-468f-8825-36ea0a1f1e25)

From the above graph, ```VIL=0.78V``` and ```VIH=1.01V ```are calculated and now, using these values from VTC we can calculate VOL and VOH.

![vol_and_voh](https://github.com/afzalamu/cmos-inverter-design-and-analysis-using-tsmc180nm/assets/124300839/1d7e1120-9611-4b48-9dc8-54c1ee356936)

from the graph, we calculated ```VOL=0.1V``` and ```VOH=1.69V```.

Now, Next is Noise Margins. Noise margins are defined as the range of values for which the device can work noise-free or with high resistance to noise. This is an important parameter for digital circuits, since they work with a set of specific values(2 for binary systems), so it becomes crucial to know what values of the voltages can sustain for each value. This range is also referred to as Noise Immunity. There are two such values of Noise margins for a binary system:
NML(Noise Margin for Low) = VIL - VOL
NMH(Noise Margin for HIGH) = VOH - VIH

So for our calculated values, the device would have, NML = 0.682V and NMH = 0.68V.

In our case, they are very accurate and equal but there can be cases where they are not the same, and as Vth can be not near to vdd/2.
Here is the summary of our results.

| Parameter | Value   |
|-----------|---------|
| Vth       | 0.903V  |
| VIL       | 0.78V   |
| VIH       | 1.01V   |
| VOL       | 0.1V    |
| VOH       | 1.69V   |














