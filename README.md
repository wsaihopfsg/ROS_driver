## ROS drivers for R2000 and R2300 laser scanners (This is still work-in-progress)

[![Build Status](https://travis-ci.org/PepperlFuchs/ROS_driver.svg?branch=master)](https://travis-ci.org/PepperlFuchs/ROS_driver)

**Required platform:**  
64-bit Windows 10 Desktop or Windows 10 IoT Enterprise

**ROS on Windows installation:**  Access http://wiki.ros.org/Installation/Windows and click on `melodic`, and follow the steps there. ![image](https://user-images.githubusercontent.com/75309631/100847609-fee64f80-34ba-11eb-9c47-96670d437385.png)

  * For Step 5, we just need 5.1 ROS Last Known Good (LKG) Build Installation because ROS 2 is not relevant to this code.

  * After Step 6 is finished, we test the installation by 
    1. Invoke the newly created ROS Command Window shortcut, and run `roscore` for a ROS master.
    2. Invoke another instance of the ROS Command Window shortcut, and run `rostopic list`. The topics published by the master will be listed. 
    3. Next try running `rviz` too. 
    
    If these can be run without issues, the ROS on Windows installation is done.

**Installing bootstrap dependencies:** Follow the steps in https://ms-iot.github.io/ROSOnWindows/Build/source.html for 

  * All build tools (compiler, CMake, etc.) installation
  * Initializing rosdep
  * Configure Chocolatey sources
  * Create a catkin Workspace
    * if there is Python runtime error `File "C:\opt\ros\melodic\x64\\lib\encodings\__init__.py", line 123` when `wstool init src melodic-desktop.rosinstall` is invoked, unset  PYTHONHOME and PYTHONPATH by
    
      ```
      set PYTHONHOME=
      set PYTHONPATH=
      ```
  * Resolving Dependencies
  
**Clone the repository:** Clone the repository in the `src` folder of your ROS workspace. For example my src folder is `C:\ros_catkin_ws\src\catkin\bin\src`
```
git clone --branch=windows https://github.com/wsaihopfsg/ROS_driver.git
```
**Install the missing dependencies:** My workspace folder is `C:\ros_catkin_ws\src\catkin\bin\src`
```
cd <path/to/workspace>
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro melodic -r -y
```
For windows, the dependencies `curlpp` and [`jsoncpp`](https://github.com/open-source-parsers/jsoncpp) are [not available and have to be downloaded by `vcpkg`](https://github.com/ros-industrial-consortium/tesseract/issues/146#issue-505378940)

```
cd\opt
git clone https://github.com/Microsoft/vcpkg.git
bootstrap-vcpkg
vcpkg integrate install
vcpkg install jsoncpp
vcpkg install curlpp
```
---
**from this point onwards a lot of trial-and-error was involved in trying to make the code compile)**

  * All of the edits were made to make the original code compilable. Please refer to the individual files for the reasons why the commits were made.

  * I am not sure why the files downloaded with vcpkg were not included, as a work-around I copied all the files from `C:\opt\ros\melodic\x64\tools\vcpkg\installed\x64-windows\include` to `C:\ros_catkin_ws\src\catkin\bin\src\ROS_driver\pf_driver\include\pf_driver\pf`. 
  
  * As a result of copying the include folder, some pre-processor at the header files are changed to absolute path with "", for example `C:\ros_catkin_ws\src\catkin\bin\src\ROS_driver\pf_driver\include\pf_driver\pf\curlpp`
  ![image](https://user-images.githubusercontent.com/75309631/100881247-a9289c00-34e8-11eb-8fda-5616cf93df33.png)
  
  * During compilation this warning occurs quite often. [To get rid of it](https://docs.microsoft.com/en-us/cpp/porting/modifying-winver-and-win32-winnt?view=msvc-160), we need to add the following pre-processor to `C:\opt\ros\melodic\x64\include\boost-1_66\boost\asio\detail\config.hpp`
    ```
    #define _WIN32_WINNT 0x0A00
    ````
    ![image](https://user-images.githubusercontent.com/75309631/100874537-b2f9d180-34df-11eb-8f08-b661e46114e1.png)

  * The code should be able to `catkin_make` and compile without error. However at linking stage, a `LINK : fatal error LNK1104: cannot open file 'curlpp.lib'` error occured despite the path of curlpp.lib being correctly displayed during the linking stage. **No idea how to solve this**
    ![error](https://user-images.githubusercontent.com/75309631/100875191-a75ada80-34e0-11eb-9f89-8ec0d4896e1f.jpg)
