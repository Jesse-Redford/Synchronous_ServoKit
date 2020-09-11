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

### Examples 
Sending incrementally increasing (periodic) velocity arguments (in rads/sec) to positional servos and having them execute the prescribed velocities over a fixed time interval (in this case 1 sec) in a synchronous fashion. Notice in the second video (non-symmetrical case), the head joint rotates at half the rate of the tail joint.

<p align="center">
<img src="https://github.com/Jesse-Redford/Synchronous_ServoKit/blob/master/synchronous_control_example.gif" width="500" height="175"> 
 <img src="https://github.com/Jesse-Redford/Synchronous_ServoKit/blob/master/varying_rates_synchronous_control_example.gif" width="500" height="175">
</p>



### Usage Example

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
      
   

### Setup Instructions 

After complettign installation and configuring your hardware import the package into your python script

        import import Synchronous_ServoKit
            
Next, create an instance for each servo in your system and configure the following parameters for each of them. 

- Reference number 
     - This will determine what order your arguments need to be in
- Channel Number
     - This will assostate what channel the servo is connected 
- Joint Limits
     - This will ensure that the servo stays within a given positonal range
- Reset position
     - The position you would like the servo to return to if the system is reset 
- Speed constant
     - Obtained from specs on servo, speed constant = t/60deg, used to approximate the delay between 1deg steps 
  
      # Define channel servo is connected to, accutation range, reset position, ect
      servo_one = Synchronous_ServoKit.configure(channel = 1, lower_servo_limit = 35, upper_servo_limit = 145, reset_position = 90, units='rads')
      servo_two = Synchronous_ServoKit.configure(channel = 2, lower_servo_limit = 35, upper_servo_limit = 145, reset_position = 90, units='rads')
      servo_three = ....
      servo_four = ....
      ect ...
  
Now constuct your system by defining a variable, in this case I will define it is "system", which is an array contained each servo instance

      system = [servo_one,servo_two,servo_three, ... ]
 
You can inspect the current system configuration by simply calling the get_info method. This will return a list of the parapemeters assosistaed with each servo 
defined in your system. 

      system_info = Synchronous_ServoKit.get_info(system)
      print(system_info)
      
You will notice that the current_positions of the servos will be listed as None, this will update after initating a system reset by simply writing
       
       # move all servos in system to there reset_positions
       Synchronous_ServoKit.reset(system)
       
       # re-check system info 
       system_info = Synchronous_ServoKit.get_info(system)
       print(system_info)
       
The current_positions of each servo should now be equal to the reset_positions you defined. You can also check the info or position of an indivdual servo by writing
       
       servo_one_info = servo_one.info
       print(servo_one_info)
       
       servo_one_position = servo_one.position
       print(servo_one_position)
       
If you want to save the current system configuration to a file just use
       
       Synchronous_ServoKit.save(system,file_name = 'Name of your system')
       
Now to reload this configuration 
       
       system = Synchronous_ServoKit.load(file_name = 'name of your system')
       
After completeting the steps above, you can now control your system using velosity arguments in a syrncounous fashion by simply writing
      
      # Get current positions of servos
      print(system.positions)
      
      # Define speeds for each servo (units deg/sec) and an execution time (units = sec) 
      speed_servo_one = 30; speed_servo_two = -30; execution_time = 1 
      arguments = [speed_servo_one,speed_servo_two,speed_servo_three,.....] 
  
      # Send arugments to be executed and get back new position 
      Synchronous_ServoKit.execute(system,arguments,execution_time)
      
      # Get new positions of servos
      print(system.positions)
      
You should verify the new positions agree with the arguments. It is recommend that you calibrate the system 

      system.calibrate(min_speed,max_speed,execution_time)




  
  
  
  
