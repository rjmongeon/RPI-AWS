# Serverless Application Design and Implementation Using Raspberry Pi and AWS
## Sensor Hardware Build

## STEP 1: Raspberry Pi Hardware Build

This is the list of materials I used to build the project. The case I used is a little different, I couldn't find the same model
| Item | Site |
| --- | --- |
| Raspberry Pi 4 Model B Kit | https://vilros.com/collections/raspberry-pi-kits/products/vilros-raspberry-pi-4-model-b-complete-starter-kit-with-clear-transparent-case-and-built-in-fan?variant=29406723768414 |
| DHT22 Temperature Humidity sensor | https://www.adafruit.com/product/385 |
| Pi prototyping header | https://www.adafruit.com/product/2029 |
| Electronic prototyping kit | https://www.jameco.com/z/PROTO-KIT-830-Point-Breadboard-with-70-Piece-Jumper-Wire-Kit-Super-Combo_2134993.html |

### Alright, when all the stuff comes in it looks like this:
![Unboxing](images/pi-unboxing.jpg?raw=true "Unboxing")

### First assemble the Pi itself. Read your case manual instructions. This is my case half assembled:
![halfdone](images/pi-almost-done.jpg "halfdone")


### Now we need to wire up our sensors. Here is an image of both the schematic and the physical wiring:
![breadboard](images/breadboard.jpg "breadboard") 

### This is the final assembly all booted and ready to go:
![booted](images/booted.jpg "booted")

## STEP 2: Install and Enable RDP on the Rpi
Once we are booted up follow any instructions on updating software. As you can see, I attached a monitor and a wireless keyboard/mouse up to the Rpi but that’s because it’s a prototype. In the real world it would be a box with a probe sticking out and 4 wires to control heat and humidity. That being said, we need to be able to remote to the Rpi for development.

Use the following guide to setup RDP on the Rpi:
http://www.circuitbasics.com/access-raspberry-pi-desktop-remote-connection/

## STEP 3: Install the Sensor drivers
* Open a terminal session and type: `sudo pip install Adafruit-DHT`
* Create a directory for our code type: `mkdir RPI`
* Open Thonny by clicking Start -> Programming -> Thonny
* In Thonny open a new file (File -> New)
* Copy the code below and paste it into Thonny

```python
import sys
import board
import Adafruit_DHT 
import time

def get_Sensor_Data():
    rc = False
    Hum = 0.0
    Temp = 0.0
    try:
        sensor = Adafruit_DHT.DHT22
        pin = 4
        Hum, Temp = Adafruit_DHT.read_retry(sensor, pin)
        rc = True
        Temp = Temp * 9/5.0 + 32
        #print("get_Data():Temp= ", Temp)
        return Hum, Temp, rc
    except RuntimeError as error:
        # Errors happen fairly often, DHT's are hard to read, just keep going
        print("runtume error caught: get_Sensor_Data() on sensor DHT22")
        return Hum, Temp, rc

while True:
    try:
        humidity, temp, result = get_Sensor_Data()
        
        if result:
            print('Temp={0:0.1f}*'.format(temp))            
            print('Humidity={0:0.1f}%'.format(humidity))  
        else:
            print("Invalid data")

        time.sleep(5)

    except (EOFError, SystemExit, KeyboardInterrupt):
        sys.exit()
    except RuntimeError as error:
        # Errors happen fairly often, DHT's are hard to read, just keep going
        print(error.args[0])

```

* Run the code in Thonny. Run -> Run current script
* Hit the red stop button after a few seconds. You should see something like this in the output:
	+ Temp=59.2*
	+ Humidity=32.2%
* If you get the results above we know the DHT22 is wired correctly. If not, check your wiring.


## STEP 4: Test the sensor's actuator
Our sensor needs to be able to turn on and off heat and humidity. We are going to simulate this on our sensor using LEDs. To program this we are going to set a couple of GPIO pins to output mode and interface them to a LED. I’m going to show just one control pin for now. We will use it to turn on a red LED. Let say the red LED represents a heating element being turned on.

If you wired the Rpi as I have shown in the pics above then you have wired a LED to a 270 ohm resistor and tied one end to pin 18, the other to ground. We can turn the LED on and OFF with a simple script. On the Rpi, open Thonny and create a new file called LED.py. Paste the following into the file and save it to the /RPI directory.

```python
#-- LED.py --
import RPi.GPIO as GPIO
import sys

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
LED = 18
GPIO.setup(LED, GPIO.OUT)
    
def LEDon():
    GPIO.output(LED,GPIO.HIGH)

def LEDoff():
    GPIO.output(LED,GPIO.LOW)

arguments = len(sys.argv) - 1

if (arguments < 1) :
    print("usage: LED on\nor LED off\npython3 LED on")

if arguments > 0:
    print("first param: ", sys.argv[1])
    if (str(sys.argv[1]) == "on") :
        LEDon()
        print("switch is on")
    if (str(sys.argv[1]) == "off") :
        LEDoff()
        print("switch is off")
#-- END LED.py –
```

* With the file saved as LED.py
* Open a terminal session and type: `cd RPI`
* Type: `sudo pip install RPi.GPIO`
* Run the script above by typing: `python LED.py on` This will turn the LED on
* Turn the LED off by typing: `python LED.py off`

If this doesn’t work check your wiring. One thing that I will add is that if the LED doesn’t light up the first time try reversing the leads. It’s a diode and as such allows current to flow only in one direction. The other thing to mention is the value of the resistor controls how bright the LED is. I originally was using a green LED with a 270 ohm resistor and it would not light up. I switched to a red LED and everything worked. Try a lower value resistor if it’s not bright enough but make sure you use a resistor because you could damage the IO buss if you are drawing too much current.

## STEP 5: Project Review
If you have followed along with me thus far we have built out our Rpi sensor and can RDP to it. My development environment is a Windows 10 laptop with dual monitors. Here it is:

![pydev](images/pi-dev.jpg "pydev")

At this point in time we now have a working sensor that we can remote to. Put it somewhere in your house and connect it to your network. You can do all the development on your Windows 10 box. In fact, if you expose the RDP port through your router you could do the rest of the development from anywhere.

# [Return](README.md)
