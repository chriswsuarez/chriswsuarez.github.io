# Check tempuratures for computer internals linux
Command line:
cat /sys/class/thermal/thermal_zone*/temp

I also wrote a script for this to make it nice and clean and easier to understand:
https://github.com/chriswsuarez/env_setup/blob/master/scripts/check_temps.sh


# Clear apt-cache and force a fresh sudo apt update
rm -rf /var/lib/apt/lists/*
sudo apt update

# Connect to usb devices regardless of their usb bus assignment (UDEV Rules)
If you have a robot with multiple usb devices they will always appear in the /dev/ directory mostly as a /ttyUSB* or /ttyACM* device.  This is the serial port assignment they are given by the computer when it boots and often these will be randomly assigned so that you never know which one will be /ttyACM0 or /ttyACM1 and so on. 

Udev rules allow you to assign a constant symbolic link to access usb devices so that you will always be able to access the same device via the same port name which you can assign.  They also let you assign proper permissions boot/ connection time as well so you don't have chmod any port after the fact.  The only items you need to know are the vendor id, the product id, and/or the serial number.  This info can be found using the dmesg command in the terminal (you will have to do some google searching on this or I will have to provide more info on here later).

    Create a .rules file in /etc/udev/rules.d/ – touch /etc/udev/rules.d/<##_MY_UDEV_RULE>.rules
    Fill out the empty file; It should look something like the following where the vendor id and product id are associated with your device. 
        The Mode is the permissions you want to give the port in this case it is like chmod 666. 
        The symlink is static port link you want the port to be assigned.
        Ensure the kernel is searching the proper port name ranges. In this case it is searching ttyUSB0 - ttyUSB9
        Ensure the group is assigned to the right user or group thing.  I don't really know how this works.


SUBSYSTEMS=="usb", KERNEL=="ttyUSB[0-9]*", ACTION=="add", ATTRS{idVendor}=="091e", ATTRS{idProduct}=="0003" MODE="666", SYMLINK+="sensors/garmin_gps", GROUP="plugdev"

Examples of udev rules: https://github.com/chriswsuarez/env_setup/udev

# Create and run a Windows VM in Linux via LXC/LXD
https://ubuntu.com/tutorials/how-to-install-a-windows-11-vm-using-lxd#3-create-a-new-vm

Once the VM container is created here are some useful commands to remember:
To start container:
lxc start <container_name> 

To connect to display once a container is started:
lxc console <container_name> --type=vga 

To stop container via terminal:
lxc stop <container_name> 

To get info and status of container:
lxc info --show-log <container_name> 


# NetworkManager config files in /etc directory
This is what I found while debugging why the Philbart has trouble switching access points on the AugRE network.

You can edit the NetworkManager profiles directly in config files in the /etc/NetworkManager/system-connections directory. To view an example I would recommend setting up a profile and then view the file once it is automatically created for you by NetworkManager.

1) My initial change for the Philbart and Jackal were to add the MAC addresses of the access points themselves to the seen-bssids  list in the AugRE.nmconnection file. Results are pending.
2) I changed the wifi powersave to off by editing /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf  to off (2).  It defaults to 3 which apparently is on. The following links explain this setting and your options with it.
https://ubuntu.com/core/docs/networkmanager/snap-configuration/wifi-powersave
https://gist.github.com/jcberthon/ea8cfe278998968ba7c5a95344bc8b55
3) Using iwconfig  you can see access point a given interface is currently connected to.


# Output command text from the terminal to a file
&> <file_name>.txt


# Place a service file to run a system service on startup
/lib/systemd/system

PS4 controller service file example:
https://github.com/chrippa/ds4drv/blob/master/systemd/ds4drv.service


# Remove app from apt lists
If you get an error when running sudo apt update specifically with one package complaining that it cannot be found or NO_PUBKEY it might be best to manually remove it from the list of apps that is being checked.

    cd /etc/apt/sources.list.d
    sudo rm <desired_package>.list 


# Start a process and keep it running after closing the terminal via tmux
Install tmux
    sudo apt install tmux

On the computer where you want to run a process that maintains operation:
    tmux
    <run your desired process>

To sign out safely from tmux while keeping the process running
    CTRL + B
    D

You can now do whatever you want with the terminal
When you are ready to get back to that process you must re-attach tmux to a terminal
    tmux attach -d -t <session id>

SEE THE CHEAT SHEET
https://tmuxcheatsheet.com/


# Setup a PS4 bluetooth controller for your robot. AKA connect bluetooth to linux
ensure bluetooth is on
systemctl status bluetooth.service

it may be blocked by rfkill
rfkill
sudo rfkill unblock bluetooth

try to scan now
bluetoothctl power on
bluetoothctl pairable on
bluetoothctl scan on

once the scan reaches near steady state then switch your controller on to pairing mode.
look for a new device that says Wireless Controller and get its MAC address
bluetooth pair <MAC address>
bluetooth trust <MAC address>
bluetooth connect <MAC address>