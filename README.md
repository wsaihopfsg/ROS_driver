## ROS drivers for R2000 and R2300 laser scanners (This is still work-in-progress)

[![Build Status](https://travis-ci.org/PepperlFuchs/ROS_driver.svg?branch=master)](https://travis-ci.org/PepperlFuchs/ROS_driver)

**Required platform:**  
64-bit Windows 10 Desktop or Windows 10 IoT Enterprise

**ROS on Windows installation**  
Access http://wiki.ros.org/Installation/Windows and click on `melodic` ![image](https://user-images.githubusercontent.com/75309631/100847609-fee64f80-34ba-11eb-9c47-96670d437385.png), and follow the steps there.

For Step 5, we just need 5.1 ROS Last Known Good (LKG) Build Installation because ROS 2 is not relevant to this code.

After Step 6 is finished, we test the installation by invoking the newly created ROS Command Window shortcut, and run `roscore` for a ROS master.

Invoke another instance of the ROS Command Window shortcut, and run `rostopic list`
