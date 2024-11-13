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
-
'''
FUNCTION_BLOCK PID (*PID controller*)
VAR_INPUT
EN : BOOL; (*Enable input for controller*)
W : REAL; (*Setpoint in percent*)
X : REAL; (*Actual percentage value*)
HA : BOOL:=0; (*Manual/automatic switch (Manual: HA=0; Automatic: HA=1)*)
KP : REAL:=1.0; (*Proportional action coefficient*)
TN : REAL:=1.0; (*Reset time*)
TV : REAL:=0.0; (*Rate timet*)
TAB : REAL:=1.0; (*Sampling time*)
YMIN : REAL:=0.0; (*Lower limit for Y*)
YMAX : REAL:=100.0; (*Upper limit for Y*)
YH : REAL; (*Manual manipulated variable percentage*)
END_VAR
VAR_OUTPUT
Y : REAL; (*Manipulated variable percentage*)
END_VAR
VAR
YP : REAL; (*P component*)
YD : REAL; (*D component*)
YI : REAL; (*Current I component*)
E : REAL; (*Error signal*)
E_OLD : REAL; (*Error signal in last cycle*)
YI_OLD : REAL; (*I component in last cycle*)
HA_OLD : BOOL:=0; (*Manual/automatic value in last cycle for edge detection*)
END_VAR
(*Start of control algorithm*)
IF EN=TRUE THEN
1 (*Calculation for error signal*)
(*next IF statement indicates positive edge detection*)
IF MA_OLD=FALSE AND HA=TRUE THEN (*Switch over from manual to automatic modes without
jerks*)
YI_OLD:=YH-KP*E-KP*TV*(E-E_OLD)/TAB-(KP*TAB/TN)*E;
END_IF;
IF HA=TRUE THEN (*Start of automatic mode*)
2 (*P component*)
IF TN>0.0 THEN (*I component*)
3
ELSE YI:=0;
END_IF;
IF TV>0.0 AND TAB>0.0 THEN (*D component*)
YD:=KP*TV*(E-E_OLD)/TAB;
E_OLD:=E; (*Save old error signal*)
ELSE YD:=0.0;
END_IF;
Y:=YP+YI+YD; (*Calculate manipulated variable*)
IF (Y>YMAX) THEN (*Anti-reset wind-up (ARW)*)
Y:=YMAX; (*Limit to YMAX*)
dinesh
YI:=YI_OLD; (*Stop integral-action*)
END_IF;
IF (Y>YMAX) THEN (*Anti-reset wind-up (ARW)*)
4 (*Limit to YMIN*)
5 (*Stop integral-action*)
END_IF;
YI_OLD:=YI; (*Save I component for new cycle*)
END_IF; (*End of automatic mode*)
IF HA=FALSE THEN
Y:=YH; (*Output manipulated variable in manual mode*)
END_IF;
MA_OLD:=MA; (*Save current manual/automatic value*)
END_IF;
END_FUNCTION_BLOCK
'''
