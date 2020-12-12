## ROS drivers for R2000 and R2300 laser scanners on Windows Subsystem for Linux (WSL) 

[![Build Status](https://travis-ci.org/PepperlFuchs/ROS_driver.svg?branch=wsl-vscode)](https://github.com/wsaihopfsg/ROS_driver/tree/wsl-vscode)

**Required platform:**  
Windows 10 (OS build 20262 or higher)
  
**AIM**  
Run and debug the code in Windows

**Install WSL2 & ROS**  
Follow the instructions for [setting up ROS in Windows through WSL2](https://jack-kawell.com/2020/06/12/ros-wsl2/)
* In the setting up GUI forwarding step, logoff from VPN if you are unable to start `xcalc`.

**Install VS Code on Windows (Not in WSL)**  
Follow the instructions here for [Visual Studio Code Remote - WSL](https://code.visualstudio.com/docs/remote/wsl)
* VS Code is to be installed on Windows. Do not install it in WSL.

**Clone the PF_driver depository in WSL**  
Create a workspace folder, and clone the ROS_driver repository in the `src` folder
```
git clone --branch=master https://github.com/PepperlFuchs/ROS_driver.git
```
**Clone the .vscode folder into workspace folder in WSL**
```
??? Copy files manually???
```
  * `c_cpp_properties.json` : IntelliSense related configurations.
    - Note that it has nothing to do with `catkin_make` parameters such as the include folders
  * `launch.json` : For roslaunch within VS Code, and target to the relevant `.launch` file when `F5` is pressed.
    - Under `configuration` -> `target`, an absolute path is needed to the launch file. Please edit it maunually.
  * `tasks.json` : For catkin_make options within VS Code, most importantly the "-DCMAKE_BUILD_TYPE=Debug" option for debugging
    - The customized task for `catkin_make` this ROS driver is defined here and can be invoked by `F1`-> `Tasks->Run Task` -> `ROS: catkin_make PF_Driver`. Note that this only performs compilation and not `roslaunch`

**Install ROS dependencies in WSL**  
```
cd <path/to/workspace>
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=melodic -y
```
**Build the workspace in WSL**  
```
cd <path/to/workspace>
source /opt/ros/melodic/setup.bash
catkin_make
source <path/to/workspace>/devel/setup.bash
```
**Invoke VS Code in WSL**  
```
cd <path/to/workspace>
code .
```
---  
(All commands are invoked within VS Code from here on)  

**Install VS Code Extensions (Ctrl-Shift-X)**  
  * C/C++ (c++ intellisense and configuration help)
  * CMake (Intellisense support in CMakeLists.txt files)
  * CMake Tools (Extended CMake support in Visual Studio Code)
  * GitLens (Git support and additional git tab)
  * Python (If you're using rospy)
  * ROS (Adds helper actions for starting the roscore, for creating ROS packages and provides syntax highlighting for .msg, .urdf and other ROS files)
  * vscode-icons (Optional, but helps with all the different file types used by ROS)
**ROS: Start Core**  
Press `F1` and invoke `ROS: Start Core`
![image](https://user-images.githubusercontent.com/75309631/101986403-907a6c00-3cc8-11eb-8311-93fdc97a4b35.png)  
You can see the ROS core status by click `ROS1.melodic` ![image](https://user-images.githubusercontent.com/75309631/101986499-15fe1c00-3cc9-11eb-9b12-b87fa7b2d926.png)
on the lower-left corner
![image](https://user-images.githubusercontent.com/75309631/101986539-56f63080-3cc9-11eb-89b4-3d0084fe0e4d.png)  
**Setting Break Points**  
You may set break points anywhere in the code. The code will be compiled (using the `ROS: catkin_make PF_Driver` task defined in `tasks.json`) and `roslaunch` with the `.launch` file supplied in the "target" in `launch.json`. The following picture shows the code at break point in line 19 of `'ros_main.cpp`, from which you may perform debugging
![image](https://user-images.githubusercontent.com/75309631/101986850-7e4dfd00-3ccb-11eb-9a75-5e47ffeb82b9.png)  
