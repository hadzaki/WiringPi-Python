#Manual Build

##Get/setup repo
```bash
git clone --recursive https://github.com/hadzaki/WiringPi-Python.git
cd WiringPi-Python
git submodule update --init
```

##Prerequisites
To rebuild the bindings
you **must** first have python-dev, python-setuptools and swig installed.
```bash
sudo apt-get install python-dev python-setuptools swig
```

##Build WiringPi
```bash
cd WiringPi
sudo ./build
```

##Generate Bindings

Return to the root directory of the repository and:

`swig2.0 -python wiringpi.i`

or

`swig3.0 -thread -python wiringpi.i`

##Build & install with

`sudo python setup.py install`

Or Python 3:

`sudo python3 setup.py install`

##Usage

	import wiringpi
	
	wiringpi.wiringPiSetup() # For sequential pin numbering, one of these MUST be called before using IO functions
	# OR
	wiringpi.wiringPiSetupSys() # For /sys/class/gpio with GPIO pin numbering
	# OR
	wiringpi.wiringPiSetupGpio() # For GPIO pin numbering


Setting up IO expanders (This example was tested on a quick2wire board with one digital IO expansion board connected via I2C):

	wiringpi.mcp23017Setup(65,0x20)
	wiringpi.pinMode(65,1)
	wiringpi.digitalWrite(65,1)

**General IO:**

	wiringpi.pinMode(6,1) # Set pin 6 to 1 ( OUTPUT )
	wiringpi.digitalWrite(6,1) # Write 1 ( HIGH ) to pin 6
	wiringpi.digitalRead(6) # Read pin 6

**Setting up a peripheral:**
WiringPi2 supports expanding your range of available "pins" by setting up a port expander. The implementation details of
your port expander will be handled transparently, and you can write to the additional pins ( starting from PIN_OFFSET >= 64 )
as if they were normal pins on the Pi.

	wiringpi.mcp23017Setup(PIN_OFFSET,I2C_ADDR)

**Soft Tone**

Hook a speaker up to your Pi and generate music with softTone. Also useful for generating frequencies for other uses such as modulating A/C.

	wiringpi.softToneCreate(PIN)
	wiringpi.softToneWrite(PIN,FREQUENCY)

**Bit shifting:**

	wiringpi.shiftOut(1,2,0,123) # Shift out 123 (b1110110, byte 0-255) to data pin 1, clock pin 2

**Serial:**

	serial = wiringpi.serialOpen('/dev/ttyAMA0',9600) # Requires device/baud and returns an ID
	wiringpi.serialPuts(serial,"hello")
	wiringpi.serialClose(serial) # Pass in ID

**Full details at:**
http://www.wiringpi.com

gpio readall

+-----+-----+----------+------+---+-Orange Pi+---+---+------+---------+-----+--+
 | BCM | wPi |   Name   | Mode | V | Physical | V | Mode | Name     | wPi | BCM |
 +-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
 |     |     |     3.3v |      |   |  1 || 2  |   |      | 5v       |     |     |
 |  12 |   8 |    SDA.0 | ALT3 | 0 |  3 || 4  |   |      | 5V       |     |     |
 |  11 |   9 |    SCL.0 | ALT3 | 0 |  5 || 6  |   |      | 0v       |     |     |
 |   6 |   7 |   GPIO.7 | ALT3 | 0 |  7 || 8  | 0 | ALT3 | TxD3     | 15  | 13  |
 |     |     |       0v |      |   |  9 || 10 | 0 | ALT3 | RxD3     | 16  | 14  |
 |   1 |   0 |     RxD2 | ALT3 | 0 | 11 || 12 | 0 | ALT3 | GPIO.1   | 1   | 110 |
 |   0 |   2 |     TxD2 | ALT3 | 0 | 13 || 14 |   |      | 0v       |     |     |
 |   3 |   3 |     CTS2 | ALT3 | 0 | 15 || 16 | 0 | ALT3 | GPIO.4   | 4   | 68  |
 |     |     |     3.3v |      |   | 17 || 18 | 0 | ALT3 | GPIO.5   | 5   | 71  |
 |  64 |  12 |     MOSI | ALT4 | 0 | 19 || 20 |   |      | 0v       |     |     |
 |  65 |  13 |     MISO | ALT4 | 0 | 21 || 22 | 0 | ALT3 | RTS2     | 6   | 2   |
 |  66 |  14 |     SCLK | ALT4 | 0 | 23 || 24 | 0 | ALT4 | CE0      | 10  | 67  |
 |     |     |       0v |      |   | 25 || 26 | 0 | ALT3 | GPIO.11  | 11  | 21  |
 |  19 |  30 |    SDA.1 | ALT3 | 0 | 27 || 28 | 0 | ALT3 | SCL.1    | 31  | 18  |
 |   7 |  21 |  GPIO.21 | ALT3 | 0 | 29 || 30 |   |      | 0v       |     |     |
 |   8 |  22 |  GPIO.22 | ALT3 | 0 | 31 || 32 | 0 | ALT3 | RTS1     | 26  | 200 |
 |   9 |  23 |  GPIO.23 | ALT3 | 0 | 33 || 34 |   |      | 0v       |     |     |
 |  10 |  24 |  GPIO.24 | ALT3 | 0 | 35 || 36 | 0 | ALT3 | CTS1     | 27  | 201 |
 |  20 |  25 |  GPIO.25 |  OUT | 0 | 37 || 38 | 0 | ALT3 | TxD1     | 28  | 198 |
 |     |     |       0v |      |   | 39 || 40 | 0 | ALT3 | RxD1     | 29  | 199 |
 +-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
 | BCM | wPi |   Name   | Mode | V | Physical | V | Mode | Name     | wPi | BCM |
 +-----+-----+----------+------+---+-Orange Pi+---+------+----------+-----+-----+
