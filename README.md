## Synchronous_ServoKit

### Project Description
An extension library for Adafruit_CircuitPython_ServoKit that allows users to control the velocity of multiple positional servos in a synchronized fashion. The project aims to provide users more sophisticated levels of servo control without needing to develop the low level code. The package acts as a stand alone interpertuer providing execution and automatic handling of the following. 

 - Sending velocity arguments in units of deg/sec or rads/sec to positional servos 
 - Moving multiple positional servos in a synchronized fashion over a fixed time interval
 - Setting upper and lower limits for servo actuation range 
 - Mapping servo positions from (0-180) deg to (-pi/2,pi/2 )radians


<p align="center">
<img src="https://github.com/Jesse-Redford/Synchronous_ServoKit/blob/master/(1)%20Process_Diagram_Synchronous_ServoKit.PNG" width="1050" height="260"> 
</p>

### Examples 
Sending incrementally increasing (periodic) velocity arguments (in rads/sec) to positional servos and having them execute the prescribed velocities over a fixed time interval (in this case 1 sec) in a synchronous fashion. Notice in the second video (non-symmetrical case), the head joint rotates at half the rate of the tail joint.

<p align="center">
<img src="https://github.com/Jesse-Redford/Synchronous_ServoKit/blob/master/synchronous_control_example.gif" width="500" height="175"> 
 <img src="https://github.com/Jesse-Redford/Synchronous_ServoKit/blob/master/varying_rates_synchronous_control_example.gif" width="500" height="175">
</p>

   ### Hardware Requrments
   - Rasberry Pi 3B+
   - adafruit 16channel servo sheild
   - 1 or more positional servos
   
   ### Installation Software Dependencies
      - pip install r.requirments.txt
      - pip install Synchronous_ServoKit
    
  <!--- ### Test 
      - cd working directiory
      - python Synchronous_ServoKit_calibrate.py 
-->


### How to use
For each servo in your system you will need to create an instance of each servo and assign the following

- Reference number 
     - This will determine what order your arguments need to be in
- Channel Number
     - This will assostate what channel the servo is connected 
- Joint Limits
  -- This will ensure that the servo stays within a given positonal range
- Reset position
  -- The position you would like the servo to return to if the system is reset 
- Speed constant
  -- Obtained from specs on servo, speed constant = t/60deg, used to approximate the delay between 1deg steps 

### Usage Examples

     import Synchronous_ServoKit
  
     # Define channel servo is connected to, accutation range, reset position, and desired units you would like to use 
     servo_one = Synchronous_ServoKit.configure(channel = 1, lower_servo_limit = 35, upper_servo_limit = 145, reset_position = 90, units='rads')
     servo_two = Synchronous_ServoKit.configure(channel = 2, lower_servo_limit = 35, upper_servo_limit = 145, reset_position = 90, units='rads')
  
     # Reset servo to there "home" reset positions
     positions = Synchronous_ServoKit.reset(servo_one,servo_two) 
  
      # Define control arguments 
      speed_servo_one = 30; speed_servo_two = 30; execution_time = 1 
      arguments = speed_servo_one,speed_servo_two,execution_time
  
      # Send arugments to be executed and get back new position 
      new_positions = Synchronous_ServoKit.execute(servo_one,servo_two,arguments)
      
   

  
  
  
  
