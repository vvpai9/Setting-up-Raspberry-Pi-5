# Setting-up-Raspberry-Pi-5

1. Using Raspberry Pi Imager, flash the Raspberry Pi OS compatible with Raspberry Pi 5 (Recommended: Rasperry Pi OS (Debian Bookworm) Full 32-bit with Desktop Environment and Recommended applications) on a SD Card (Recommended: Class 10 32GB Micro SD Card).
 
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

e) When prompted, select no to 'Would you like a login shell to be accessible over serial?'

f) When prompted, select yes to 'Would you like the serial port hardware to be enabled?'.

g) Reboot the Raspberry Pi using ```sudo reboot``` when you are done.

h) The Raspberry Pi’s serial port will now be usable on ```/dev/ttyAMA0```.

4. Run the following commands:

(Some of these dependencies come pre-installed with the Debian Bookworm OS. In such cases, you will get a message saying ```Requirement already satisfied```)
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

(```picamera2``` library is required in Debian Bookworm if you are using a Raspberry Pi Camera)
```
pip install future
pip install lxml
pip install picamera2
```

```
pip install pyserial
pip install dronekit
pip install geopy
pip install MAVProxy
```

If the second set of installations are throwing an error repeatedly, try the following command:
```
pip cache purge
pip install pyserial dronekit geopy MAVProxy --index-url https://pypi.org/simple
```

7. Install OpenCV within the virtual environment:
```
sudo apt-get install python3-opencv
```
8. Check OpenCV installation:
```
pi@raspberrypi:- $ python3
Python 3.12.3 (tags/v3.12.3:f6650f9, Apr  9 2024, 14:05:25) [MSC v.1938 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
```
The ```cv2 ``` module should be imported into Python without any errors. This indicates that OpenCV module is correctly installed. If any error like ```ImportError: numpy.core.multiarray failed to import``` is generated, run the following command within the virtual environment:
```
pip uninstall numpy
```

If you want to activate the virtual environment everytime the terminal is opened, go to ```nano ~/.bashrc``` and add the following line at the end:
```
source ~/myDrone/bin/activate
```

Save the file and exit the text editor (in ```nano```, you do this by pressing CTRL + X, then Y, and Enter).

To apply the changes immediately without needing to restart the terminal, run:
```
source ~/.bashrc
```

To deactivate the virtual environment when not required, run:
```
deactivate
```


# Integrate Raspberry Pi with PixHawk
1. Set following parameters in PixHawk through Mission Planner:


```SERIAL2_PROTOCOL = 2```

```SERIAL2_BAUD = 921```

```LOG_BACKEND_TYPE = 3```



2. Connect PixHawk and Raspberry Pi as shown in the figure:

![f837b6b1116ec02c3490e34035c2f09da5a62936](https://github.com/user-attachments/assets/7dee1fc9-4551-4b20-94bb-4c6c462b59b1)


3. Power the Raspberry Pi using BEC module. Make sure that the power supply used is atleast 5V/3A (Recommended: 5V/5A (25 W to 27W)). Power supply less than 5V/3A may cause performance issues or the Pi may end up abruptly crashing or shutting down.

a) Check port
```
ls /dev/ttyAMA0
```


b) Add the following two lines at bottom of file ```sudo nano /boot/firmware/config.txt``` :
```
enable_uart=1
dtoverlay=disable-bt
```
Save the file and exit the text editor (in ```nano```, you do this by pressing CTRL + X, then Y, and Enter).

4. Now type the following to get the telemetry data of Pixhawk:
```
mavproxy.py --master=/dev/ttyAMA0 --baudrate 921600
```

5. Type the following if you want telemetry data to be displayed in Mission Planner:
```
mavproxy.py --master=/dev/serial0 --baudrate 921600 --out udp:127.0.0.1:14552

/*Here,
 '127.0.0.1' Your PC's IP Adress, Obtained by typing 'ipconfig' in command prompt
 '14552' is the port to which you need to connect to mission planner using UDP
*/
```
