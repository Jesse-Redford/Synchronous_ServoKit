# Synchronous_ServoKit

#### Project Description
An extension library for Adafruit_CircuitPython_ServoKit that allows users to control the velocity of multiple positional servos in a synchronized fashion. The project aims to provide usesr more sophisticated levels of servo control without needing to develop the low level code. 

#### Features
- Acts as a stand alone interpertuer class which provides execution and automatic handling other the following. 
  - Sending velocity arguments in units of deg/sec or rads/sec to positional servos 
  - Moving multiple positional servos in a synchronized fashion over a fixed time interval
  - Setting upper and lower limits for servo actuation range 
  - Mapping servo positions from (0-180)deg to (-pi/2,pi/2)radians
  

#### Software Dependencies and Requirments
 - pip install r.requirments.txt
  

#### Usage Example

  """
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
  
   """
   

  
  
  
  