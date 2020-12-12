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
Create a workspace folder, and clone the repository in the `src` folder
