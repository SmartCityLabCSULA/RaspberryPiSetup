# RaspberryPiSetup

Setup information for the Raspberry Pis

## About

The image that we're using (provided by [ROSbots](http://www.rosbots.com/)) has Raspbian Stretch Lite (no GUI) with the 
[Robot Operating System (ROS)](http://www.ros.org/) Kinetic and [OpenCV 3.4.1](http://opencv.org/) installed.

Note that this image is for the Raspberry Pi 3 and has only been tested (by ROSbots) for the Raspberry Pi 3 and the 
Raspberry Pi 3 B+. Since we're using the Raspberry Pi Zero W, we're in uncharted territory.

## Setup

1. Download the image
    - Go to [this link](http://bit.ly/2x2NVnq) to download the image. You will probably have to fill out a quick survey. The 
    survey's only required questions are
        - How would you describe yourself?
        - What country are you located in? If USA, what state?
        - Are you building a robot from a robot kit? If YES, what kit? 

2. Write the image to the SD card
    - Using whatever program you like ([Etcher](https://etcher.io/) seems like a good option), write the image to the 
    SD card.

3. Start up the Pi
    - The first time you start the Pi up, you're going to want to have a physical connection (ethernet or keyboard and mouse) 
    with it.
    - If you're using a network connection via ethernet (`ssh`) you'll need a way to find the Pi's IP address. You can use 
    Nmap for this by running `sudo nmap -sP x.x.x.0/24` where `x.x.x.0` is your network subnet (this is for a Debian-based 
    system, for [Windows](https://nmap.org/download.html) you'll have to find the appropriate method).

4. Login
    - username: pi
    - password: rosbots!

5. Run `initialize_rosbots_image` from the command line. This will
    1. Setup new `ssh` keys
    2. Ask you for a new password
    3. Run `raspi-config` which will allow you to expand the filesystem to fill the entire SD card

6. With `raspi-config`, you can configure the Wi-Fi and expand the filesystem
    1. To expand the filesystems
        - Select _Advanced Options_ -> _Expand Filesystems_
    2. To configure the Wi-Fi
        - Select _Network Options_
        - Enter the `ssid` and passcode
    4. Select _Finish_
    5. Reboot the Pi

7. After the Pi has restarted and you've logged in (using the new password), you can run `rosnode list` to see that ROS is
running. This is not what we want! You will need to modify the `.bashrc` file to prevent this and remove the `rosbots` script
to stop `roscore` from starting at start up
    1. `nano ~/.bashrc`
    2. Scroll down to the bottom and change `export ROSBOTS_MASTER=1` to `export ROSBOTS_MASTER=0`
    3. Press `Ctrl-x` then press `y` to save the changes
    4. `sudo rm /etc/init.d/rosbots` to remove the `roscore` start up script
    5. Restart the Pi by running `sudo reboot`
8. Profit!
