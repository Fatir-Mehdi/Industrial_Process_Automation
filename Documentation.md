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
(_______________________________________)

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

### Actual Experiment Procedure

