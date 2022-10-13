# delivery_robot_phase_1
Implementation of phase 1 of the delivery robot. Integrated the hardware with an application and database for basic navigation of bot. Bot takes delivery commands from the user through the app and moves accordingly. Status - Completed

## Software requirements

### Operating system
Tested on Ubuntu 18.04

### Robotic Operating System
Ros Melodic

## Raspberry Pi set up

## Prepare SD card
1. For SD cards with more than 32GB size, the file system is exFAT and not FAT32. But RPi does not recognize exFAT, hence change it to FAT32 to work. Used a [3rd party](https://www.diskpart.com/download-home.html) software to do this for our 64GB SD card.
2. Use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to flash Ubuntu Server 20.04 LTS to the SD card.
3. Before flashing, do ctrl+shift+x to open image customization options. There we can set the hostname, enable SSH and configure wifi. This is done for headless set up.
4. After flashing, insert SD card into the slot in Raspberry Pi and connect the power supply. Red LED indicates sufficient power. Blinking green LED indicates active Pi.

## Access Raspberry Pi remotely

Make sure the Raspberry Pi and the PC are connected to the same network.

1. Install necessary tools
```
sudo apt install nmap
sudo apt-get install openssh-client
sudo apt-get install openssh-server
```
2. To get the lcal IP address
```
hostname -I
```
3. Replace the last number of the local IP address with 0/24 to the subnet range (Eg: if the local IP address is 192.168.43.68 then...)
```
nmap -sn 192.168.43.0/24
```
From this, we will get the IP address of Raspberry Pi.
4. - To establish an SSH connection using the IP address of Raspberry Pi, username and password we will be able to access the Pi remotely
```
ssh <username>@<ip_address>
```
Resource: [How to Find the IP Address of your Raspberry Pi](https://raspberryexpert.com/find-raspberry-pi-ip-address/)

## Create a catkin workspace
Refer [this ](http://wiki.ros.org/catkin/Tutorials/create_a_workspace) tutorial.

## Setting up a local repository
1. Create a ROS package in the catkin workspace
    ```
    cd ~/<catkin_workspace_name>/src
    ```
    ```
    catkin_create_pkg delivery_robot_phase_1 std_msgs rospy roscpp
    ```
    ```
    cd ~/<catkin_workspace_name>
    ```
    And then execute ```catkin build``` or ```catkin_make``` command.
2. To get catkin working after ROS installation execute
```
sudo apt install python3-catkin-tools python3-osrf-pycommon
```

3. Initialise git <br />
Make sure you have configured git using the commands.
    ```
    git config --global user.email "you@example.com"
    git config --global user.name "your name"
    ```
    Initialise git in the local repository by executing the following commands.
    ```
    cd ~/<catkin_workspace_name>/src/delivery_robot_phase_1
    ```
    ```
    git init
    ```
    ```
    git add .
    ```
    ```
    git commit -m "Created ROS package" .
    ```
    Rename master branch to main
    ```
    git branch -m main
    ```

4.  Connect local repository to remote
    ```
    git remote add origin https://github.com/MIT-Mentors/delivery_robot_phase_1.git
    ```
    ```
    git pull origin main --allow-unrelated-histories
    ```

## Install WiringPi library

WiringPi is a Cpp library for Raspberry Pi. With this library you can use many of the functionalities provided by the GPIO header: digital pins, SPI, I2C, UART, etc.

### Installation
Refer [this](http://wiringpi.com/download-and-install/) for installation.

### Modifications to be done
Add the following lines to CMakeLists.txt
```
find_library(WIRINGPI_LIBRARIES NAMES wiringPi)
target_link_libraries(<executable_name> ${WIRINGPI_LIBRARIES})
```

## Pin mapping details

GPIO 4 - Front right motor PWM
GPIO 5 - Front right motor direction
GPIO 2 - Front left motor PWM
GPIO 3 - Front left motor direction
GPIO 28 - Rear right motor PWM
GPIO 29 - Rear right motor direction
GPIO 24 - Rear left motor PWM
GPIO 25 - Rear left motor direction

## To run the code
Execute
```
roscore
```
In another terminal execute
```
roslaunch delivery.launch
```