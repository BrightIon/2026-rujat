# Advanced PID Topics
Here is where you'll add your mini-report. Fill in (or create) a new section with a 2nd level heading `##My Topic` and use markdown formatting. 
Images can be included by adding the image file to /imgs and referencing it `![my-label](imgs/my_image.png)`.

## Markdown Example - Turbo Encabulator

#### Issue: 

The first version of the machine had a base-plate of prefabulated aluminite, surmounted by a malleable logarithmic casing in such a way that the **2** main spurving bearings were in a direct line with the pentametric fan. 


The latter consisted simply of **six** hydrocoptic marzlevanes, so fitted to the ambifacient lunar waneshaft that side fumbling was effectively prevented. 

```py
#lotus-o-delta algorithm
for i in range(7):
    tremie_pipe += gridlespring + 0.5*marzlevane[i]
```
#### Solution:

The main winding was of the normal lotus-o-delta type placed in panendermic semi-bovoid slots in the stator, every **seventh** conductor being connected by a non-reversible tremie pipe to the differential girdlespring on the "_up_" end of the grammeters.

![turbo](imgs/turboencabulator.png)


## Sample Time
#### Issue: 
Unlike analogue circuits, digital circuits operate on the basis of some timed clock, where updates to the system occur regularly, instead of instantaneously. A specific sampling time is required to time updates in the PID controller.
#### Solution:

Several approaches to solve this issue exist. An approach suggested by Brett [1] works as follows:
1. Define a variable `SampleTime`.
2. When the PID controller function is called, it checks whether the difference between current time `now` and the previous time `lastTime` is greater than or equal to the `SampleTime`, it will update the output of the PID controller. And hence, it updates the error.
3. Otherwise, it does nothing.

The code below implements the process described above.
```cpp
unsigned long lastTime;
double Input, Output, Setpoint;
double errSum, lastErr;
double kp, ki, kd;
int SampleTime = 1000; //1 sec
void Compute()
{
   unsigned long now = millis();
   int timeChange = (now - lastTime);
   if(timeChange>=SampleTime)
   {
      /*Compute all the working error variables*/
      double error = Setpoint - Input;
      errSum += error;
      double dErr = (error - lastErr);
 
      /*Compute PID Output*/
      Output = kp * error + ki * errSum + kd * dErr;
 
      /*Remember some variables for next time*/
      lastErr = error;
      lastTime = now;
   }
}
```

The problem with the code above is that it relies on the user calling the compute function in a timely manner to begin evaluating the error. This could be avoided by utilizing an interrupt module/systemcall to have the function `Compute` called at regular specific time intervals, eliminating the need for the user to time the calling. The updated code below has the `newCompute` function that shall be linked to an interrupt within the operating system/microcontroller you are using.

```cpp
unsigned long lastTime;
double Input, Output, Setpoint;
double errSum, lastErr;
double kp, ki, kd;
void newCompute()
{
    /*Compute all the working error variables*/
    double error = Setpoint - Input;
    errSum += error;
    double dErr = (error - lastErr);

    /*Compute PID Output*/
    Output = kp * error + ki * errSum + kd * dErr;

    /*Remember some variables for next time*/
    lastErr = error;
    lastTime = now;
}
```


## Derivative Kick
#### Issue: 

#### Solution:

## Tuning Changes
#### Issue: 

#### Solution:

## Reset Windup
#### Issue: 

#### Solution:

## Manual toggle
#### Issue: 

#### Solution:

## Initialisation
#### Issue: 
If the PID controller is toggled off, when it is turned back on again the controller will incorrectly try to correct to the previous set-point. If the set point has changed this results in an unintended bump where the PID controller moves the wrong way. 

(add image)

#### Solution:
This unintended spike can be prevented by reinitialising upon the change from manual to automatic. For a smooth transition, the input term is updated to the new input, and the integral term is set to the output. This reinitialises the PID controller and allows for seamless switching between the manual and automatic modes. 

```py
void Initialize()
{
   lastInput = Input;
   ITerm = Output;
   if(ITerm> outMax) ITerm= outMax;
   else if(ITerm< outMin) ITerm= outMin;
}
```


## 'Proportional on Measurement'

#### What is PonM?
Proportional on Measurement, sometimes abbreviated to *PonM*.

In a standard PID, the proportional *'P'* term acts on the error, as: 
$$

e(t)=r(t) - y(t)

$$
where $r(t)$

#### Issue: 


#### Solution:


