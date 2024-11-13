# Industrial_Process_Automation
This **Compact Station project** has provided me with practical experience in process automation and PLC-based control systems. Through hands-on experiments, I’ve learned to design, program, and tune PID controllers for applications like level, pressure, and temperature control, gaining valuable skills in controller programming, parameter adjustment, and system troubleshooting. This project has connected my theoretical knowledge to real-world applications, reinforcing my problem-solving and analytical abilities in automation engineering. The experience has been crucial in preparing me for roles in process industries, enhancing my technical and practical expertise in control systems.

![image](https://github.com/user-attachments/assets/d2524b7a-f43e-49a1-a244-8cb64d9b04c6)

1 - Tank B102
2 - Tank B101
3 - Pump P100
4 - Variable-area flow meter
5 - Electric flow sensor
6 - UniTrain system
7 - Ultrasonic filling-level sensor, pressure sensor, safety valve
8 - Water reservoir B100

## Methodology
1. System Setup and Familiarization: Describe the experimental setup, listing key components like the UniTrain system, experiment cards, sensors, actuators, and the compact station itself. Explain the function of each component and refer to the P&I diagram for a visual representation of the plant layout and process flow.

2. Controller Programming: State that a custom PID control algorithm will be programmed on the PLC, referencing the input and output parameters table from the source and explaining their significance. Include the programming code for the algorithm, emphasizing anti-reset wind-up and its implementation.

3. Controller Testing: Explain the testing procedure for the programmed controller, including why initial tests with a simulated control loop are beneficial. Describe how to use virtual instruments and the step-response plotter for testing, providing instructions for both manual and automatic modes.

4. Experimental Determination of PID Control Parameters: State the goal of finding optimal control parameters (Kp, TN, and TV) and introduce the inflectional tangent and time-based percentage methods. Refer to the detailed explanations in the source and, if using LabSoft, mention its parameter calculation features.

5. Practical Application on the Compact Station: Select a specific process (e.g., level, pressure, or temperature control) from the source's experiments. Define the control objective, describe the flow path setup, provide wiring details, and outline steps for conducting the experiment. Guide the user on observing and interpreting the system's response.

6. Data Analysis and Conclusion: Instruct the user to record data (setpoints, actual values, and manipulated variable output) and utilize tools like the time-trace plotter for visualization. Guide the analysis to evaluate the controller's effectiveness, discuss the influence of control parameters, and draw conclusions about the practical implementation of PID control. Emphasize safety precautions, sensor calibration, exploration of different tuning rules, and the importance of observing system behavior.

## Tools and Resources 

Sensors:
*Flow Sensor:* Measures the flow rate of water using an impeller and inductive proximity switch.
*Variable-Area Flow Meter (Rotameter):* Provides on-site indication of flow rate based on the height of a float in a tube.
*Pressure Sensor:* Measures the pressure above atmospheric pressure using a ceramic cell transducer.
*Manometer:* Offers on-site pressure indication.
*Ultrasonic Sensor:* Measures the filling level in a tank based on the time it takes for ultrasonic waves to reflect back to the sensor.
*Capacitive Filling-Level Sensor:* A binary sensor that detects the presence of liquid at a specific point in the tank.
*Temperature Sensor and Evaluation Unit:* Consists of a Pt100 resistance thermometer and an evaluation unit that outputs a standard voltage signal proportional to the temperature.

Actuators:
*Pump:* A self-priming, dual-membrane pump with speed control via a PWM signal.
*Heater:* An electric heater operating in on/off mode.
Valves:
*3-Way Manual Valves:* Used for manually controlling the flow path of water.
*Safety Valve:* Relieves pressure from the tank.

Additional Tools Used:
*PLC Programming Software:* Used to program the custom PID control algorithm and other logic for the experiments. The sources do not specify the exact software used, so you may want to note that in your documentation.
*Virtual Instruments:* These software tools simulate real-world instruments and allow for setting input values and monitoring output variables.
*Step-Response Plotter:* This tool records the system's response to a step change in the manipulated variable, aiding in the determination of control parameters.
*Time-Trace Plotter:* Visualizes the controller's performance over time by plotting the setpoint and actual values of the controlled variable.
*Excel Form ("CONTROLLER_e.xls"):* Facilitates the calculation of PI and PID control parameters using various tuning rules based on the measured step response data.

## Experiment Brief:

Here are the various experiments listed in the document, in the same sequence:
**Filling vessel B101
Automatic control of water level
Recording step response
Controller set-up and testing
Automatic pressure control
Taking into account the derivative-action component
Automatic flow rate control
Instability of the control loop
Automatic temperature control using two-position controller
Test of controller**

For Detailed Documentation go to: [Brief Description](Documentation.md)

## Results

**Level Control**
The step response characteristics were determined as follows: t10 = 15 s, t50 = 40 s, t90 = 80 s, Δxe = 20%, Δy = 45%.
Applying the “Modulus optimum” tuning rule, the calculated PI controller parameters were Kp = 9.6 and TN = 46 s.
In automatic mode, the water level successfully reached and maintained the setpoint of 30%, as evidenced by the time-trace plot (Figure 1). The system exhibited a slight overshoot before settling. Opening valve V104 caused a decrease in the controlled variable, but the controller effectively compensated and returned the level to the setpoint.

**Pressure Control**
Due to the near-linear initial portion of the step response curve, the time-based percentage method was utilized, yielding: t10 = 5 s, t50 = 18 s, t90 = 35 s, Δxe = 70%, Δy = 50%.
The Excel form "CONTROLLER_e.xls" with the "Modulus optimum" rule resulted in the following PI parameters: Kp = 8 and TN = 20 s.
The controller effectively maintained the pressure at the setpoint of 30% in automatic mode. Opening valve V104 induced a disturbance, causing a pressure drop. The controller responded and restored the pressure to the setpoint, demonstrating the effectiveness of the PI control (Figure 2). The inclusion of the derivative action component (TV = 0.5 s) resulted in a slightly more aggressive response but introduced minor oscillations in the manipulated variable.

**Flow Rate Control**
The inflectional tangent method provided the following controller parameters: Kp = 2.5 and TN = 1.2 s.
In automatic mode, the flow rate stabilized around the setpoint of 30%, although some fluctuations were observed, likely due to the pulsating nature of the pump or the flow sensor characteristics (Figure 3). Increasing Kp beyond 6 resulted in instability, with oscillations appearing in both the manipulated and controlled variables.

**Temperature Control**
The two-position controller, with a hysteresis of 1°C, successfully maintained the temperature within ±1°C of the 40°C setpoint. Increasing the hysteresis to 5°C resulted in a lower switching frequency and a wider fluctuation range (±5°C) around the setpoint.

## Skills Acquired

Project Planning: Designing and planning automated control systems using PLCs for various industrial processes.
Closed-Loop Control: Implementing continuous closed-loop control strategies, a foundational concept in process automation.
Two-Position Control: Applying two-position controllers and analyzing their switching operations, especially in temperature control applications.
Controller Parameter Determination: Experimentally determining control parameters (Kp, TN, TV) for continuous controllers using techniques like the inflectional tangent and time-based percentage methods.
Controller Operation and Monitoring: Operating, monitoring, and troubleshooting controllers and processes using virtual instruments and step response plotters.
P&I Diagram Interpretation: Understanding piping and instrumentation diagrams (P&IDs) for analyzing process control systems.
PLC Programming: Applying PLC programming skills to implement control algorithms and system logic.
Process Control Fundamentals: Reinforcing process control basics, including familiarity with temperature, pressure, and flow rate measurement.
Measurement of Non-Electrical Variables: Measuring non-electrical variables such as temperature, pressure, and flow rate.
Analog and Digital Converters: Utilizing analog-to-digital and digital-to-analog converters for process control and automation.

## Importance of the Project
This project has been valuable in developing essential skills directly relevant to process industries like chemical, pharmaceutical, and manufacturing sectors. This project has deepened my understanding of automation engineering principles, particularly in designing, implementing, and maintaining industrial control systems. Through hands-on tasks like controller tuning and troubleshooting, I've strengthened my problem-solving and analytical abilities. The practical experience has also provided me with a solid foundation in applying theoretical knowledge to real-world scenarios, bridging the gap between academic learning and industry practices.

## Conclusion
This practical exercise provided valuable insights into the principles and implementation of PID control using a PLC system. The experiments successfully demonstrated the effectiveness of PI control in regulating level, pressure, and flow rate. The results validated the theoretical concepts of PID control, where the integral action component ensured elimination of steady-state error. The influence of the proportional and derivative action parameters on the system's response was also observed. Notably, the time-based percentage method proved more reliable than the inflectional tangent method for pressure control due to the initial linearity of the step response curve. The temperature control experiment highlighted the characteristics of two-position control, showcasing the trade-off between switching frequency and fluctuation range as determined by the hysteresis value.
This practical experience reinforced the importance of proper controller tuning and the selection of appropriate control strategies based on the system's dynamic behavior. The observed instability in the flow rate control loop when Kp exceeded a critical value emphasized the need for careful parameter adjustment to avoid detrimental oscillations.
Further investigation could explore the impact of different tuning rules on the controller performance and examine the effects of noise and disturbances on the control loop stability. Implementing more complex control schemes, such as cascade control, could provide additional challenges and learning opportunities for future practical work.

