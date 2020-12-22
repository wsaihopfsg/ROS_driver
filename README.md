## ROS drivers for R2000 and R2300 laser scanners on Windows Subsystem for Linux (WSL) 
[![Build Status](https://travis-ci.org/PepperlFuchs/ROS_driver.svg?branch=wsl-vscode)](https://github.com/wsaihopfsg/ROS_driver/tree/wsl-vscode)

**Required platform:**  
Windows 10 (OS build 20262 or higher), Pro, Enterprise or Windows Server versions
  
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
**Pretty Printing**  
To enable pretty printing, [invoke `-exec -enable-pretty-printing` at `DEBUG CONSOLE`](https://qiita.com/taka_horibe/items/50e4659e88d1ee3cd04a)
![image](https://user-images.githubusercontent.com/75309631/102900638-89611400-44a7-11eb-9a75-125810dce17e.png)  

---  
**Solved Issue: Nothing displayed in RViz**  
Symptoms:
  * From Wireshark, can see that the R2300 (192.168.0.78) is sending packets to WSL (192.168.202.83)
    ![image](https://user-images.githubusercontent.com/75309631/102000901-23e28a00-3d27-11eb-8c11-0f9c7297cc1f.png)
  * The packets seem to be valid, starting with the magic byte 0xa25c
    ![image](https://user-images.githubusercontent.com/75309631/102000918-4a082a00-3d27-11eb-8cff-d63c2ff80451.png)
  * However no activity is observed in the Hyper-V WSL2 adapter, probably [due to this](https://docs.microsoft.com/en-us/windows/wsl/compare-versions#accessing-a-wsl-2-distribution-from-your-local-area-network-lan)
    - can ping from WSL2 to another pc
    - can ping from Gateway to WSL2
    - cannot ping from another pc to WSL2  
    ![image](https://user-images.githubusercontent.com/75309631/102061375-5f1bb080-3e2e-11eb-9ee6-fadcf629fb32.png)
  * [This video](https://www.youtube.com/watch?v=yCK3easuYm4) suggests to use `netsh` to do a v4tov4 port forward, but it only works for TCP
    ![image](https://user-images.githubusercontent.com/75309631/102082801-8a61c800-3e4d-11eb-9ab1-3dfdec305e1c.png)  
**Solution:**
  * Since my laptop is on Windows Enterprise, I can manipulate the Hyper-V settings to make it run in bridge mode. The details can be found in [here](https://github.com/microsoft/WSL/issues/4150#issuecomment-747152240)  
  ![image](https://user-images.githubusercontent.com/75309631/102197120-b0de3c80-3efb-11eb-9914-cf481ef990c4.png)


