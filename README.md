# Industrial_Process_Automation
Simple continuous closed-loop control Two-position controllers and switching operations Experimental determination of control parameters for continuous controllers Operation and monitoring of controllers and processes/plants Representing process control systems using piping and instrumentation flow diagrams.

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


