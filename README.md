## ROS drivers for R2000 and R2300 laser scanners (This is still work-in-progress)

[![Build Status](https://travis-ci.org/PepperlFuchs/ROS_driver.svg?branch=master)](https://travis-ci.org/PepperlFuchs/ROS_driver)

**Required platform:**  
64-bit Windows 10 Desktop or Windows 10 IoT Enterprise

**ROS on Windows installation:**  Access http://wiki.ros.org/Installation/Windows and click on `melodic`, and follow the steps there. ![image](https://user-images.githubusercontent.com/75309631/100847609-fee64f80-34ba-11eb-9c47-96670d437385.png)

For Step 5, we just need 5.1 ROS Last Known Good (LKG) Build Installation because ROS 2 is not relevant to this code.

After Step 6 is finished, we test the installation by invoking the newly created ROS Command Window shortcut, and run `roscore` for a ROS master.

Invoke another instance of the ROS Command Window shortcut, and run `rostopic list`. The topics published by the master will be listed. Next try running `rviz` too. If these can be run without issues, the ROS on Windows installation is done.

**Installing bootstrap dependencies:** Follow the steps in https://ms-iot.github.io/ROSOnWindows/Build/source.html for 

  * All build tools (compiler, CMake, etc.) installation
  * Initializing rosdep
  * Configure Chocolatey sources
  * Create a catkin Workspace
  * Resolving Dependencies
  
**Clone the repository:** Clone the repository in the `src` folder of your ROS workspace. For example my src folder is `C:\ros_catkin_ws\src\catkin\bin\src`
```
git clone --branch=master https://github.com/PepperlFuchs/ROS_driver.git
```
