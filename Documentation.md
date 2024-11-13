# PI Diagram for the Project is as follows:
![image](https://github.com/user-attachments/assets/fa9d0203-39ae-4a3d-a39a-b7ce7170303b)

## Description

### Programming your own closed-loop controller

The objective is to create a fully functioning PID control algorithm, even though the actual
experiments will only use it for a PI controller. This controller function module will eventually look like
the following:
![image](https://github.com/user-attachments/assets/d0e052a0-65b8-4e6c-a3c7-136db8ea948a)

![Screenshot 2024-11-13 183852](https://github.com/user-attachments/assets/ff705f90-d6d0-472d-baab-1d4296c1eef0)
![Screenshot 2024-11-13 183924](https://github.com/user-attachments/assets/7c1cdc2e-eeb6-4132-9ece-3c61b540454d)

P component: yp,k=Kp · ek
I component: yI,k=yI,k-1+KI · ek; KI=Kp · Tab/TN
D component: yD,k=KD · (ek-ek-1)/Tab; KD=Kp · TV
The overall value for the manipulated variable is therefore given by the following: yk=yp,k+yI,k+yD,k
In this representation, k is the corresponding process cycle period.

Function Block Program for the Controller:

FUNCTION_BLOCK PID (* PID controller *)
VAR_INPUT
    EN : BOOL; (* Enable input for controller *)
    W : REAL; (* Setpoint in percent *)
    X : REAL; (* Actual percentage value *)
    HA : BOOL := 0; (* Manual/automatic switch (Manual: HA=0; Automatic: HA=1) *)
    KP : REAL := 1.0; (* Proportional action coefficient *)
    TN : REAL := 1.0; (* Reset time *)
    TV : REAL := 0.0; (* Rate time *)
    TAB : REAL := 1.0; (* Sampling time *)
    YMIN : REAL := 0.0; (* Lower limit for Y *)
    YMAX : REAL := 100.0; (* Upper limit for Y *)
    YH : REAL; (* Manual manipulated variable percentage *)
END_VAR

VAR_OUTPUT
    Y : REAL; (* Manipulated variable percentage *)
END_VAR

VAR
    YP : REAL; (* P component *)
    YD : REAL; (* D component *)
    YI : REAL; (* Current I component *)
    E : REAL; (* Error signal *)
    E_OLD : REAL; (* Error signal in last cycle *)
    YI_OLD : REAL; (* I component in last cycle *)
    HA_OLD : BOOL := 0; (* Manual/automatic value in last cycle for edge detection *)
END_VAR

(* Start of control algorithm *)
IF EN = TRUE THEN

    (* Calculation for error signal *)
    E := W - X;

    (* Positive edge detection for switch from manual to automatic mode without jerks *)
    IF HA_OLD = FALSE AND HA = TRUE THEN
        YI_OLD := YH - KP * E - KP * TV * (E - E_OLD) / TAB - (KP * TAB / TN) * E;
    END_IF;

    IF HA = TRUE THEN
        (* Automatic mode *)
        
        (* P component *)
        YP := KP * E;

        (* I component *)
        IF TN > 0.0 THEN
            YI := YI_OLD + (KP * TAB / TN) * E;
        ELSE
            YI := 0;
        END_IF;

        (* D component *)
        IF TV > 0.0 AND TAB > 0.0 THEN
            YD := KP * TV * (E - E_OLD) / TAB;
            E_OLD := E; (* Save old error signal *)
        ELSE
            YD := 0.0;
        END_IF;

        (* Calculate manipulated variable *)
        Y := YP + YI + YD;

        (* Anti-reset wind-up (ARW) for YMAX *)
        IF Y > YMAX THEN
            Y := YMAX; (* Limit to YMAX *)
            YI := YI_OLD; (* Stop integral action *)
        END_IF;

        (* Anti-reset wind-up (ARW) for YMIN *)
        IF Y < YMIN THEN
            Y := YMIN; (* Limit to YMIN *)
            YI := YI_OLD; (* Stop integral action *)
        END_IF;

        (* Save I component for next cycle *)
        YI_OLD := YI;
    END_IF; (* End of automatic mode *)

    IF HA = FALSE THEN
        (* Manual mode *)
        Y := YH; (* Output manipulated variable in manual mode *)
    END_IF;

    (* Save current manual/automatic value *)
    HA_OLD := HA;

END_IF;

END_FUNCTION_BLOCK


### Testing the Controller

Save the linked file TEMP_CO4204-3E_ENU_IPA1_Compact station in a local folder on
your PC.
Open the File menu and select the option Open Workspace... .
Click the "Load Template..." button.
Navigate to the folder location.
Select the file that you just have saved.
Open the template "PID ".
Now finish off the programming for the control function module as described on the
previous step.

### Experimental ascertainment of PID control parameters OR Tuning the PID Controller

The sources explain that simply setting up a PID controller isn't enough to guarantee optimal performance. The effectiveness of a PID controller hinges on selecting the right control parameters: Kp (proportional gain), TN (reset time), and TV (rate time). The chapter highlights a critical finding from a study in the US process engineering industry: a surprisingly large number of control loops using PI(D) controllers performed subpar or required manual operation. The key takeaway is that neglecting this experimental tuning process often leads to inefficient control and forces operators to intervene manually.

Tuning Methods:
The chapter introduces two primary methods for experimentally determining PID control parameters:
Inflectional Tangent Method:
This method involves recording the system's step response and analyzing the resulting curve.
The user draws a tangent at the curve's inflection point (the point where the curvature changes direction).
The intersections of this tangent with horizontal lines representing zero and the maximum value provide time values used to calculate the control parameters.
The LabSoft software offers a tool to simplify this process by automatically calculating parameters based on the Ziegler-Nichols method or the Chien, Hrones, and Reswick method after the user draws the tangent.

![image](https://github.com/user-attachments/assets/ff80009b-4f2f-4ae0-b32e-5be6ad1ea427)

![image](https://github.com/user-attachments/assets/37474d96-f84f-481e-aa85-3a85604bb410)

element with a time constant of 20 seconds and a gain of 1 is recorded.
To draw the inflectional tangent, it is very helpful to use the zoom function. It is easier to detect the
point of inflection at higher resolutions.
If you click “Evaluation” on the tool bar, you can calculate the parameters using either the Ziegler-
Nichols method or the Chien, Hrones and Reswick method.

Time-Based Percentage Method:
This method analyzes specific points on the step response curve to determine characteristic values. Here's a breakdown of the steps:
Step 1: Measurement and Analysis: The process begins by switching the controller to manual mode and letting the controlled variable stabilize. Next, the manual manipulated variable is changed by a specific step amount (Δy), and the system's response is recorded until the controlled variable stabilizes again. The user then needs to determine the following from the recorded curve:
Δy: The step change magnitude applied to the manipulated variable.
Δxe = xe - xa: The total change in the controlled variable's value during the step response.
t10, t50, t90: The times it takes for the controlled variable to reach 10%, 50%, and 90% of Δxe, respectively. These time values are also called time-percentage characteristic values.
Step 2: Conversion to Percentage: Since the control algorithm within the PLC works with percentage values, the change in the controlled variable (Δxe), which is typically in physical units (like °C), needs to be converted into a percentage (Δxe%) relative to the sensor's measuring range.
Step 3: Calculation Using Excel: The gathered data—Δy, Δxe%, t10, t50, and t90—are then entered into a provided Excel form. This form utilizes various tuning rules to automatically calculate the PI and PID control parameters based on the input data. The sources recommend using the "absolute value optimum" tuning rule for achieving the best control response in most applications.

Step 1: 

![image](https://github.com/user-attachments/assets/c42582a2-dd00-420e-8651-657eb0c02dad)

help of the touch panel; for this purpose, the curves can be evaluated easily using the software
measurement tool . The time periods t10, t50 and t90 determined in this manner are also termed
time-percentage characteristic values.

Step 2:
Δxe% = Δxe 100/ (xE - xA).
As described earlier:
Δxe = change in the step response in physical units
Δxe% = percentage change in the step response
xE = end of the measuring range
xA = start of the measuring range

Step 3:

![image](https://github.com/user-attachments/assets/fd8ddd85-296f-4661-91e5-08af0d207ef5)

Practical Example for above Tuning Method:
Automatic temperature control of a drum Dryer

![image](https://github.com/user-attachments/assets/54b55561-380e-46b9-a540-65023ceea523)

1: Input product
2: Hot gas
3: Gas burner
4: Drum dryer
5: Outer jacket
6: PI controller
7: Output product
x: Output product's temperature
w: Set-point value for x
y: Gas valve's manipulated variable
The input product (1) is placed in the rotating drum dryer (4). A gas burner (3) heats the dryer via its
outer jacket. The plant is meant to output a dry product (7) of a certain quality. The output product's
temperature x is a measure of the quality. Accordingly, a PI temperature controller (6) is installed to
ensure the desired product temperature w via the gas valve's manipulated variable y.

Step 1:
The chart below shows the controlled system's step response as measured from an operating point
as well as the evaluation data. At the operating point, the gas valve's manipulated variable is
changed by
Δy = -3.33%
The temperature sensor's measuring range is as follows:
xA = 0°C,
xE = 400°C.

![image](https://github.com/user-attachments/assets/2029a9ab-7703-47c6-9389-465ffce8de04)

Step 2:
As described in step 2, the variable Δxe = -55°C needs to be converted into a percentage with
respect to the measurement range. Accordingly:
Δxe%= Δxe 100/ (xE - xA) = -55°C 100/400°C = -13.75%

Step 3:
Enter the following evaluation data into the EXCEL form CONTROLLER.XLS.
Δy= -3.33%,
Δxe% = -13.75 %
t10 = 0.46 h
t50 = 0.89 h
t90 = 1.53 h
PI control parameters are then calculated according to the "modulus optimum" tuning rule, for
instance.

### Actual Experiment Procedure | Compact Station

#### Project 01 | Filling vessel B101

First we shall fill the lower vessel B101 from the lower reservoir B100. B101 will then itself be the
reservoir for all future experiments. The final water level in B101 should be below the higher of the
two binary sensors L4 so that pump P100 does not turn off automatically.
The following illustration shows how the flow paths need to be set up

![image](https://github.com/user-attachments/assets/4449ae4e-f820-4c2a-bb79-8f72280fd06b)

The image includes the following aspects:
The flow paths to be set, shown in blue
The setting of the blue lever for the three-way valve.
The state of the manual valve.

Save the linked file TEMP_CO4204-3E_ENU_IPA1_Compact station in a local folder on
your PC.
Open the File menu and select the option Open Workspace... .
Click the "Load Template..." button.
Navigate to the folder location.
Select the file that you just have saved.
Open the workspace "Filling".
Compile the program by pressing the symbol , under Compiler menu → Compile
program or F7.
Set the PLC to RUN mode and turn on the 24-V supply.
Set the input for enabling the pump (IX 0.6).
Set a speed for the pump using the DC source virtual instrument.

--> Automatic control of water level

This section covers implementation of a water level control system using a PI controller.
The illustration below shows the required paths for the flow of water in blue.


![image](https://github.com/user-attachments/assets/6a6c317e-bc53-48bf-964c-26213d66fec4)

For this level control exercise, vessel B101 is to be used as a reservoir for filling vessel B102. B102
has a drainage outlet.

The controller requires the inputs to be connected as shown below.


![image](https://github.com/user-attachments/assets/c4d37df2-5c83-41ea-ad6a-edd4829f5d37)


![image](https://github.com/user-attachments/assets/c9db9a82-4d46-4bd9-bdc4-19a41e6c5ab1)

100.0-the actual value is considered instead of the actual value itself, because the value from
the ultrasonic sensor is at its highest when the vessel is empty and falls as the level rises.

Save the linked file TEMP_CO4204-3E_ENU_IPA1_Compact station in a local folder on
your PC.
Open the File menu and select the option Open Workspace... .
Click the "Load Template..." button.
Navigate to the folder location.
Select the file that you just have saved.
Open the corresponding workspace.
Open the virtual instrument CONTROLLER MONITOR
Select the step response view.
Open the Parameter menu and make the following settings for the control monitor.


![image](https://github.com/user-attachments/assets/a60d8de7-f433-4473-9969-ecc06413155a)


![image](https://github.com/user-attachments/assets/6269e0ab-549f-49e0-80b0-ba94fbf1d1e7)

Recording Step Response:
The next thing to do is to record a step response for the control loop and use the adjustment rules for
the loop to identify the appropriate parameters.
Start with the hardware set-up from the previous page and add the wiring shown below. These
connections are required for activating the step and reading off the actual value.

program or F7.
Set the PLC to RUN mode and turn on the 24-V supply.
Input switch I_HA (IX0.7) needs to be at 0 position to set the controller to manual mode.
Empty vessel B102.
Close the manual valve V104 and then open it by turning it three full revolutions anticlockwise.
Valve V106 needs to be open.
Set the input for enabling the pump (IX 0.6).
Now start the step response plotter . The measurement will take 10 minutes.

The Curve:

![image](https://github.com/user-attachments/assets/7553b575-aeff-4ba5-bfc9-f511a4f2a080)

Draw the inflectional tangent onto the plot. It is easier to draw the tangent if you magnify
the first part of the curve.
Use "Evaluate" to display the controller parameters using the Chien, Hrones and Reswick
method. Set up an aperiodic system with optimisation for disturbances.
Copy your results into the placeholder below.

Controller Set-up and Testing:
Enter values for Kp and TN into your program
Compile the program by pressing the symbol , under Compiler menu → Compile
program or F7.
Set the PLC system back into RUN mode.
Swap over the connection from the analog output of the UniTrain systems from the
second analog input of the PLC card to the third one .
Input switch I_HA (IX0.7) must be set to 1 for the controller to operate in automatic mode.
Now you can change the controller set-point in automatic mode. First you can use the DC
source virtual instrument to input a value and observe how the water level changes in the
system.

#### Project 02 | Automatic Pressure Control

This section covers implementing pressure control using a PI controller.
The illustration below shows the required paths for the flow of water in blue.

![image](https://github.com/user-attachments/assets/022fd4ad-93b5-4f1b-9625-928c7fbd5788)

For the pressure control experiment, vessel B101 is used a s a reservoir feeding vessel B102. B102
has a drainage outlet.

The only thing to have changed from the level control experiment is that a connection for the
pressure level goes to the first input on the PLC unit instead of one for the filling level
(volume).

![image](https://github.com/user-attachments/assets/f240e4af-7ec2-4132-9eef-aa7e6e0ddb2c)


![image](https://github.com/user-attachments/assets/50bb1e55-c1bc-433a-97ac-1d88d2e2163c)


Save the linked file TEMP_CO4204-3E_ENU_IPA1_Compact station in a local folder on
your PC.
Open the File menu and select the option Open Workspace... .
Click the "Load Template..." button.
Navigate to the folder location.
Select the file that you just have saved.
Open the corresponding workspace.
Open the virtual instrument CONTROLLER MONITOR.
Select the step response view.
Open the Parameter menu and make the following settings for the control monitor.

![image](https://github.com/user-attachments/assets/d1d11e61-8e0f-4c03-96ed-8c381b01fb0b)

Recording Step Response

The next thing to do is to record a step response for the control loop and use the adjustment rules for
the loop to identify the appropriate parameters.
Start with the hardware set-up from the previous page and add the wiring shown below. These
connections are required for activating the step and reading off the actual value.

Compile the program by pressing the symbol , under Compiler menu → Compile
program or F7.
Set the PLC to RUN mode and turn on the 24-V supply.
Input switch I_HA (IX0.7) needs to be at 0 position to set the controller to manual mode.
Empty vessel B102.
Close valve V106 so that the pressure in the vessel can build up.
Close the manual valve V104.
Set the input for enabling the pump (IX 0.6).
Now start the step response plotter.

![image](https://github.com/user-attachments/assets/8290daae-07c1-4d98-97d0-509a24661e3c)


The curve starts off almost linear, which makes it difficult to draw an inflectional
tangent.Therefore the evaluation should be made using the time-based percentage
method

Controller Set-up and Testing 
Enter values for Kp and TN into your program.
Compile the program by pressing the symbol , under Compiler menu → Compile
program or F7.
Set the PLC system back into RUN mode.
Swap over the connection from the analog output of the UniTrain systems from the
second analog input of the PLC card to the third one.
Input switch I_HA (IX0.7) must be set to 1 for the controller to operate in automatic mode.
Now you can change the controller setpoint in automatic mode. First you can use the DC
source virtual instrument to input a value and observe how the pressure level changes in
the system.

![image](https://github.com/user-attachments/assets/b2a07072-1aca-4951-9b95-29c52512e616)


Start the time trace plotter.
Next, open V104 by about half a turn to simulate a disturbance. This causes the
controlled variable to decrease.

#### Project 03 | Automatic Flow Rate Control

This section covers implementing flow rate control using a PI controller.
The illustration below shows the required paths for the flow of water in blue

![image](https://github.com/user-attachments/assets/0f85b901-359b-4567-bbfa-0701b5ba4040)


In this case, flow rate control is implemented by pumping water out of the vessel, B101, which you
filled previously. Float sensor F1 can also measure flow readings in a range from 0 ... 3 l/min.

The only thing to have changed from the pressure control experiment is that a connection for
the volume flow goes to the first input on the PLC unit instead of one for the pressure.

![image](https://github.com/user-attachments/assets/42e50f7f-d0b2-4d0b-81dc-d59459cfb738)


![image](https://github.com/user-attachments/assets/740f0330-7e79-4c20-a7e0-194e17407dfd)


Save the linked file TEMP_CO4204-3E_ENU_IPA1_Compact station in a local folder on
your PC.
Open the File menu and select the option Open Workspace... .
Click the "Load Template..." button.
Navigate to the folder location.
Select the file that you just have saved.
Open the corresponding workspace.
Open the virtual instrument CONTROLLER MONITOR.
Select the step response view.
Open the Parameter menu and make the following settings for the control monitor.

![image](https://github.com/user-attachments/assets/ffc237c6-3ced-4e76-82cc-f97ee0995a83)


Recording Step Response 

Compile the program by pressing the symbol , under Compiler menu → Compile
program or F7.
Set the PLC to RUN mode and turn on the 24-V supply.
Input switch I_HA (IX0.7) needs to be at 0 position to set the controller to manual mode.
Set the input for enabling the pump (IX 0.6).
Now start the step response plotter.

You should record the step response twice, since the first time there is likely to be air in the pipes,
which would cause incorrect readings.
After making the recording, you can set up the display properties as appropriate. Since the step
occurs very quickly, you only need to consider the first part of the curve, so that you can zoom in on
that area for better resolution when you are drawing in the inflectional tangent.

![image](https://github.com/user-attachments/assets/d84f85ff-44c9-40ba-b7fb-840345d48f0c)


Draw the inflectional tangent for the curve.
Let the computer work out the controller parameters and display for a Chien, Hrones and
Reswick configuration. Select an aperiodic time-base, optimized for the reference
variable.

Controller Set-up and Testing 

Enter values for Kp and TN into your program
Compile the program by pressing the symbol , under Compiler menu → Compile
program or F7.
Set the PLC system back into RUN mode.
Swap over the connection from the analog output of the UniTrain systems from the
second analog input to the third one.
Input switch I_HA (IX0.7) must be set to 1 for the controller to operate in automatic mode.
Now you can change the controller setpoint in automatic mode. First you can use the DC
source virtual instrument to input a value and observe how the flow through the system
changes.

![image](https://github.com/user-attachments/assets/b3ef3ae2-0136-4abf-a7ef-de913f4fbddb)

#### Project 04 | Automatic temperature control using two-position controller

In this section, you are to control the temperature of vessel B101. Since the actuator (heater E101)
can only be on or off, the controller only needs to be a switch controller. We will use a twoposition
controller for this, as explained on the following pages.

The illustration below shows the required paths for the flow of water in blue.
Automatic temperature control using two-position controller

![image](https://github.com/user-attachments/assets/5cfc608a-2ca3-482c-85fe-03403c7d4bc9)


While the temperature is being controlled, the reservoir should continually have water pumped
through it in order to keep the contents well mixed.

The following illustration shows the basic principle of a two-position controller. The name comes from
the fact that such a controller can only assume two possible states, either on or off.

![image](https://github.com/user-attachments/assets/2882248d-d999-44af-ab96-2809e8104216)


Key:
y: Manipulated variable,
e: System deviation or error signal,
H:Degree of hysteresis,
ON: On state,
OFF: Off state.
As you can see from the above characteristic curve, compares the defined setpoint with the value
currently detected for the controlled variable and works out the deviation e. If the deviation is large
enough, the controller turns on and when the deviation is sufficiently small, the controller turns off.
Hysteresis is a physical term meaning how long an action continues to have an effect after cessation
of the cause. In this case it gives rise to a relatively large reduction in switching frequency as
opposed to case in which there were no hysteresis (i.e. H = 0), thus reducing stress on the actuator
circuit. In practice, when settled, the controlled variable x fluctuates by an amount ±H around the
setpoint value w. You will be able to verify this later.
As the difference e gets larger, the hysteresis curve follows its lower path, whereas as it
reduces, hysteresis takes the upper path.

Controller:

![image](https://github.com/user-attachments/assets/601fe4b6-0dfd-4205-b935-e9c4d7e824af)

![image](https://github.com/user-attachments/assets/69a21710-93fd-45d9-b79e-b8800818703f)


Save the linked file TEMP_CO4204-3E_ENU_IPA1_Compact station in a local folder on
your PC.
Open the File menu and select the option Open Workspace... .
Click the "Load Template..." button.
Navigate to the folder location.
Select the file that you just have saved.
Open the workspace "Temperature".
First finish off the programming for the controller module as described above.
Compile the program by pressing the symbol , under Compiler menu → Compile
program or F7.

Considerations:
The binary output for the heater control does not go straight to the heating system but turns on
an additional hardware switch (not shown) which prevents the heating control reaching temperatures
(see the section containing the flow diagram for the system).
The the algorithm for a two-position controller does not take account of a sampling period Tsp, the
EN input to the controller can be permanently fixed to TRUE.
Since the measuring range of the temperature sensor is 0°C ... 100°C and the variables are
converted into percentages, the numeric values as a percentage coincidentally match the
temperature values in °C for this controller.
The following additional factors need to be worked into the programming:
The heating control should only be enabled when the pump is running.
When pumping water out, the pump should be activated at a voltage of 10 V.
The heating will be turned off by the software, if the temperature is detected to be higher
than 55°C.
