# SACI Temperature Controller

SACI (Seu Amigo Cervejeiro Inteligente or Your Smart Brew Buddy in a quick translation) is a NODEMCU 1.0 (ESP8266 12-E)
temperature controller for homebrewing and beer fermentation.

SACI was built using inexpensive parts available in any eletronic store (see the BoM and schematics bellow)
and software was writen in C/C++ using arduino/genuino framework, so SACI is easy to build,
and easier to hack.

### Reliable behavior with new brains
If you are a homebrewer, the chances are you already know and relie on ON/OFF controllers (STC-1000 is a  world wide example) to ferment your beer. SACI uses the same logic for control temperature and offers new possibilites, like data logging with IoT free services ( MatLab thingspeak.com ), automatic profile, responsive web interface and the chance to write your own plugin or interface.   

### Automatic profile
SACI was built with **set and forget mindset**. You plug it in your fridge, adjust target temperature,
create a profile for the next two weeks and done. You can do other tasks and SACI will evaluate
time using NTP and adjust your target temperature based on a previous profile.

### Clean and Responsive Web Interface
We don´t like messing interfaces, so we choose to keep it clean and 100% browser ready.
SACI has an responsive interface, beautiful in any device. Nothing to install, just point
your PC or smartphone to SACI IP address and configure it.

### Over the air updates
Put your SACI behind your freezer and not in the mood push it just to update interface?
No problem. SACI has Over The Air enabled at boot, so you can code, do your job and
update it without cables or take your notebook to brew area.

### Arduino/Genuino IDE Compatible
You already use Arduino/Genuino Framework to write your own code and want write your own plugin using it?
No problem. The code is compatible with Arduino/ESP8266 language and IDE. No additional software required.   

### Easy to build and hack
Anyone armed with soldering wire and a dozen of components can build and modify SACI hardware, and,
because of arduino/genuino framework, everyone can write plugins in a couple of hours.
In fact, SACI already has plugins for MatLab Thingspeak services, influxDB time series database
and a draft for Rsyslog integration (Some work is needed here).

### HTTP POST config and json responses
All the configuration is passed to SACI over URL encoded post, so, if you know how to manipulate this, you are able to create your own interface e/or dashboard to view your brew status.

### Plugin Ready
Already have a working code for **<insert your favorite IoT provider here>** and
want to put it in SACI?

Just get your code and put it inside plugins directory and insert your function inside main loop,
and your plugin is ready to work. SACI was sectioned to enclose things to need be enclosed together,
creating a easy to maintain and less fragmented code.

# Hardware Build Instructions
### Schematics
Shame on me here!
I don´t have the skills to write eletronic diagrams and schematics for other people, and don´t have time or will to overcome this. To be honest, I barelly can read my own schematics. If you decide to build your own PCB, you will found the etcheable layout, a totally messed up Fritzing file and this JPG file to help you with wiring diagram. Any help to improve this will be over-happily  welcomed. :)

![Image of wiring diagram](http://drive.google.com/uc?export=view&id=0B24GFOLAe1WGemEtNUNNVUNDblk)

**PS: Works on 127v or 220v**


### Bill Of Materials
- 1x nodeMCU 1.0
- 2x 15 pin female header (single row)
- 2x BC546 NPN Transistors or equivalent
- 2x 1n4148 Zenner Diode  or equivalent
- 2x 5V coil relay (10A) or SSR
- 2x 2 pin screw terminals
- 1x 3 pin screw terminal
- 1x 4.7K resistor (1/4W)
- 1x DS18B20 temperature sensor
- 2x power sockets for fridge and heat element
- 1x plastic case of your preference (or none...)
- Materials for soldering, PCB build and wires

# Software Build Instructions
### Dependencies
- [OneWire](https://github.com/PaulStoffregen/OneWire)
- [DallasTemperature](https://github.com/milesburton/Arduino-Temperature-Control-Library)
- [NtpClient](https://github.com/arduino-libraries/NTPClient)

### PlatformIO
- Clone this repository
- On PlatformIO menu, click on "Open Project Folder" and select repository folder
- On PlatformIO menu, click on "Initialize or Update PlatformIO Project"
- Choose NodeMCU 1.0 and click process
- Update your platformio.ini file as bellow

```
[env:nodemcuv2]
platform = espressif8266
framework = arduino
board = nodemcuv2
build_flags = -Wl,-Tesp8266.flash.4m1m.ld
#Change to IP to use OTA updates
upload_port = COM6
board_f_cpu = 160000000L
lib_deps =
  OneWire
  DallasTemperature
  NTPClient
```
- Create your 'src/config.h' file (use 'src/config.sample.h' as example)
- Build, deploy and make beer!! PlatformIO will handle any dependencies for you

### Arduino IDE
- Install ESP8266 support as described [here](https://github.com/esp8266/Arduino)
- Install the dependencies using Arduino Library Manager or download and by yourself
- Rename 'saci-temperature-controller/src/main.cpp' to 'src.ino'
- Copy all h and cpp files from plugins folder to src folder
- Open src.ino file
- Create and edit config.h file (use config.sample.h as example)
- Edit all paths on cpp and h plugin files (boring, i know...)
- Select NodeMCU 1.0 as board
- Select 160Mhz CPU Frequency
- Select Flash Size 4M (3M SPIFFS)
- Select Upload speed 115200
- Build the software
- Deploy firmware and filesystem using OTA ou USB port

### CLI
Need to be done. Any help will be appreciate.

# Help and todo
- Improve software build instructions
- Write eletronic diagrams that people can read and follow
- Review hardware and schematics to improve safety
- Test any CLI build mode
- Fix typos and spelling errors
- Save and load profiles by name
- Fix / Clean up / review code
- Test and write instructions for Farenheit temperature
- Test or port the code for atmel hardware (arduino/genuino based boards)
- Run other algorithms outside the hardware (slave mode)
- Improve HTTP api
- Add continuous integration
- Add unit tests
