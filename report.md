# Quest Name
Authors: Andrew Brownback, Timin Kanani, Mark Vinciguerra

Date: 2022-05-03
-----

## Summary

In this project we enhanced the features we had previously developed on our buggy, by tweaking the coefficients in our proportional integral differential steering equation so that it could follow the track more consistently. We also enhanced the emergency stopping functionality so that lidar sensor on the car would effectively determine if an object in front of the car was too close. The additional features we added were using UDP to query to the user on their laptop if the car encountered an obstacle, it would ask the user to input different letters asking what it should do - turn left, turn right, or keep going forward. 


## Self-Assessment

- Mark 95
- Timin 95
- Drew 95 

### Objective Criteria

| Objective Criterion | Rating | Max Value  | 
|---------------------------------------------|:-----------:|:---------:|
| Objective One |  |  1     | 
| Objective Two |  |  1     | 
| Objective Three |  |  1     | 
| Objective Four |  |  1     | 
| Objective Five |  |  1     | 
| Objective Six |  |  1     | 
| Objective Seven |  |  1     | 


### Qualitative Criteria

| Qualitative Criterion | Rating | Max Value  | 
|---------------------------------------------|:-----------:|:---------:|
| Quality of solution |  |  5     | 
| Quality of report.md including use of graphics |  |  3     | 
| Quality of code reporting |  |  3     | 
| Quality of video presentation |  |  3     | 

## Investigative Question

If the user is to query, and send the car instructions, then we could determine which direction to turn by using two more distance sensors angled on the left and right side of the buggy facing outward so that they could measure if one side was closer than the other (assuming the wall/course follows the turn) then this would indicate to turn the direction opposite of the closer distance. If the wall does not follow the turn then it could maybe use GPS to know where to go. Otherwise the safest option would be, if the car does not know what to do, would be to simply stop and wait for a user query. 


## Solution Design

--> Speed sensing
In order to measure the speed of our car we implemented a optical encoder and fixed a black and white pattern to the wheel. The optical encoder could then indicate a voltage pulse on a transition from black to white, and using the esspressif pulse counting library we could count the pulses. Then we were able convert the amount of pulses per second to a speed using the circumfrence of the wheel as well as the number of transitions from black to white on our pattern(6.)

--> Distance sensing
To measure the distance in various directions, we incorporated three sensors. The first being the Garmin v4 Lidar, which was fixed to the front of the car and measures distance of object in front which allows the car to stop or speed up, given something might be in the way or not. We chose lidar for this sensor since it draws a lot of power and we only needed 1 on the front anyway. Also it is by far the most accurate. We needed to measure the distances on the left and right of the car in order to determine when steering was necessary. For this we used IR-rangefinders, since they are more accurate than ultrasonic and easy to incorporate through ADC pins. Also, their range (20-150cm) fit what we needed well. 

--> PID for Steering
In order to adjust speed and steering proportionally to the error from the setpoint detected by our sensors, we used PID. This was especially important for the steering since it was not difficult to maintain a constant speed. Without PID for the steering, if the car became too close to the wall, it would steer away from the wall but then it would be too far away and steer back at the same intensity/angle. This led the car to drive in an S shape and with increasingly dramatic turns until it hit the wall. By implementing PID, we could simply adjust the steering based on the integral of the error measured in the past and therefore not steer too hard or very hard based on the previous cumulative error. 

--> UDP Server
The UDP server was implemented using the example code combined with our very first skill which uses UART to receive and send inputs from the keyboard as instructions to the car. 

## Sketches and Photos
<center><img src="./images/ece444.png" width="25%" /></center>  
<center> </center>


## Supporting Artifacts
- [Link to code](https://github.com/BU-EC444/Team11-Team-Hobbit-Brownback-Vinciguerra-Kanani/tree/master/quest-6/code).


## Modules, Tools, Source Used Including Attribution

The below links aided us in completing this and the previous quests

## References
https://github.com/espressif/esp-idf/tree/master/examples/protocols/sockets/udp_server

https://learn.sparkfun.com/tutorials/qrd1114-optical-detector-hookup-guide#example-circuit

https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/pcnt.html

https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/gptimer.html

https://github.com/BU-EC444/bu-ec444-whizzer/blob/Spring-2022/images/encoder.gif

https://github.com/BU-EC444/bu-ec444-whizzer/blob/Spring-2022/briefs/design-patterns/dp-pid.md

http://static.garmin.com/pumac/LIDAR-Lite%20LED%20v4%20Instructions_EN-US.pdf

https://github.com/garmin/LIDARLite_Arduino_Library

https://github.com/espressif/esp-idf/tree/master/examples/peripherals/i2c/i2c_simple

https://github.com/BU-EC444/bu-ec444-whizzer/blob/Spring-2022/briefs/design-patterns/dp-esc-buggy.md

https://www.hobbywing.com/products/enpdf/QuicRunWP1625-WP860-WP1060.pdf

https://p11.secure.hostingprod.com/@hobbypartz.com/ssl/ibuyrc/manual/51C852.pdf



-----

