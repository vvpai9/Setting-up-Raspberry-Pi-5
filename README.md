# Setting-up-Raspberry-Pi-5
1. Using Raspberry Pi Imager, flash the Raspberry Pi OS compatible with RPi 5 (Debian Bookworm) on a SD Card.
2. Access the Raspberry Pi through Wi-Fi via SSH
3. Set up serial connection and type the following in SSH:
```
sudo raspi-config
```
4. Change the folowing settings:

a) Go to interface settings

b) Enable SSH

c) Enable VNC

d) Go to serial

e) When prompted, select yes to 'Would you like a login shell to be accessible over serial?'

f) When prompted, select yes to 'Would you like the serial port hardware to be enabled?'.

g) Reboot the Raspberry Pi when you are done.

h) The Raspberry Pi’s serial port will now be usable on ```/dev/ttyAMA0```.

4. Run the following commands:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3-pip
sudo apt-get install python-dev
sudo apt-get install screen
sudo apt-get install python-wxgtk4.*
sudo apt-get install libxml2-dev
sudo apt-get install libxslt1-dev
```
5. Create a virtual environment to install Python packages:
```
python3 -m venv myDrone
source myDrone/bin/activate
```
6. Install required Python packages:
```
pip install future
pip install lxml
pip install pyserial
pip install dronekit
pip install geopy
pip install MAVProxy
```
7. Install OpenCV within the virtual environment:
```
sudo apt-get install python3-opencv
```
