This section briefly describes the employed sensors and actuators.

**Flow Sensor**

![image](https://github.com/user-attachments/assets/a5dad6a7-eb76-4952-8e8f-dab3c13cb486)

Used to measure flow rate as part of flow control, the flow sensor contains an impeller which rotates
in dependence on the flow speed: The rotational frequency is proportional to the flow rate. Measured
by means of an inductive proximity switch, the rotational frequency is converted into a standard 0 ...
10 V electric signal. The measuring range is 0 ... 60 dl/min.

**Variable-area flow meter (rotameter)**

![image](https://github.com/user-attachments/assets/44ed43d1-a06d-47ee-b852-7518d4c4b20a)

The variable-area flow meter is also used for flow measurement and enables on-site indication of
flow. A float is suspended inside a tube through which liquid flows. The float's height serves as a
measure of the flow rate. The flow meter has a throttle and a measuring range of 0 ... 3 l/min.

**Pressure sensor**

![image](https://github.com/user-attachments/assets/c2022b02-0a82-40c2-8eb5-91db33ac8712)


This sensor measures the pressure above atmospheric pressure. The measurement quantity is
needed for pressure control of the B102. The sensor's transducer consists of a ceramic cell. The
sensor outputs a standard 0 ... 10 V signal. The measuring range is 0 ... 1 bars.

**Manometer**

![image](https://github.com/user-attachments/assets/4220afd0-cc56-4f61-bad6-34a4f8091158)


Also for measuring the pressure above atmospheric pressure, the manometer is used for on-site
indication and has a range of -1 ... 1.5 bars.

**Ultrasonic sensor**

![image](https://github.com/user-attachments/assets/305a6987-55c9-4740-ab6a-cf829d507fa8)

The ultrasonic sensor is used for filling-level measurement and control. Its measuring principle
involves a transmission of ultrasonic waves which are reflected by the target surface (water in this
case). The sensor measures the time taken by the reflected waves to strike the sensor again. The
travelling time is proportional to the distance to the target surface and thus a measure of the filling
level. The ultrasonic sensor has a measuring range of up to 250 mm, which corresponds to a
standard 10 V signal. The sensor needs to be calibrated before use in experiments.

**Capacitive filling-level sensor**

![image](https://github.com/user-attachments/assets/c906f4f3-b821-435e-b741-23ef29835724)


The capacitive filling-level sensor is a binary sensor. It is mounted on the outside of the tank for
contactless detection of liquid at this point in the tank. Measurements are based on evaluations of
changes in capacitance caused by a presence of liquid. This sensor is used here for alarm issue and
safety shut-down of the pump and heater.

**Temperature sensor and evaluation unit**

![image](https://github.com/user-attachments/assets/1eddbfee-1f34-475b-bfb2-5d888dbf2eb9)

The temperature sensor comprises a Pt100 resistance thermometer meant for connection to the
illustrated evaluation unit. This unit generates a standard, 0 ... 10 V signal in the range from 0 ... 100°
C. It also possesses a contact which switches at 60°C and serves for safety shutdown of the heater
here.

**Pump and controller**

![image](https://github.com/user-attachments/assets/8b5f7023-a96e-4361-a467-3d93431fbcea)


The pump incorporates a self-priming, dual-membrane mechanism. It is protected against dry running
and has an integrated 1.2 bar pressure switch. The pump's speed is adjusted via a 0 ... 10 V signal
fed to the illustrated pump controller. This in turn generates an appropriate (PWM) control signal for
the motor.

**Heater**

![image](https://github.com/user-attachments/assets/01026090-0fa7-4b3e-93b1-9ada7a2cea31)


The heater has a power of 1000 W and is operated in on/off mode with 230 V AC. A semiconductor
relay's contact is used for voltage switching.

**3-way manual valve**

![image](https://github.com/user-attachments/assets/d8187cba-fb39-4c06-8756-a5bfc18ba571)


This valve is used for manual flow path control by means of the illustrated blue lever. The required
lever settings are specified in all the experiment procedures.

**Safety valve**

![image](https://github.com/user-attachments/assets/9d4b8ae5-7383-4265-a3e4-c5572f908f92)


This valve is used for pressure relief of tank B102. The valve opens at an overpressure of 1 bar to
relieve B102.


### PLC Pin Assignment

![image](https://github.com/user-attachments/assets/d93ce171-4f9f-497a-a5c0-35dbb7735e27)


### Measuring Range of the Sensors Used:

![image](https://github.com/user-attachments/assets/e99b9048-f754-44e7-8a42-de53af26f34c)
