📚 ESP32 LCD Book Reader
A handheld e-reader built with an ESP32, 20×4 I2C LCD, SD card module, and analog joystick. Read .txt books from an SD card with bookmark saving to flash memory.

📋 Table of Contents

Hardware Requirements
Wiring
Software Setup
Libraries
SD Card Setup
Uploading the Code
Usage
Troubleshooting


Hardware Requirements
ComponentQuantityESP32 Development Board120×4 I2C LCD Display1SD Card Module (SPI)1Analog Joystick Module1Breadboard + Jumper Wires—MicroSD Card (FAT32)1

Wiring
SD Card Module → ESP32
SD Module PinESP32 GPIOCSGPIO 5MOSIGPIO 23MISOGPIO 19SCKGPIO 18VCC3.3VGNDGND
LCD (I2C) → ESP32
LCD PinESP32 GPIOSDAGPIO 21SCLGPIO 22VCC3.3VGNDGND
Joystick Module → ESP32
Joystick PinESP32 GPIOVRx (X-axis)GPIO 34VRy (Y-axis)GPIO 35SW (Button)GPIO 27VCC3.3VGNDGND

Note: GPIOs 34 and 35 are input-only pins on the ESP32 — this is fine for the joystick's analog axes.


Software Setup
1. Install Arduino IDE
Download and install the Arduino IDE.
2. Add ESP32 Board Support

Open Arduino IDE and go to File → Preferences
Paste the following URL into "Additional Board Manager URLs":

   https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json

Go to Tools → Board → Board Manager
Search for esp32 and install esp32 by Espressif Systems

3. Select Your Board

Tools → Board → ESP32 Arduino → ESP32 Dev Module
Tools → Port → select your COM port


Libraries
Install via Library Manager
(Tools → Manage Libraries)
LibraryAuthorSearch TermLiquidCrystal I2CFrank de BrabanderLiquidCrystal I2C

⚠️ There are several similarly named libraries — make sure you select Frank de Brabander's version.

Pre-installed with ESP32 Board Package
(No installation needed)
LibraryPurposeSPI.hSPI communication for SD cardSD.hSD card read/writeWire.hI2C communication for LCDPreferences.hBookmark saving to ESP32 flash memory

SD Card Setup

Format your SD card as FAT32
Copy your .txt book files to the root directory of the card
Both .txt and .TXT extensions are supported

SD Card Root/
├── moby-dick.txt
├── dracula.txt
└── frankenstein.txt

Uploading the Code

Clone or download this repository
Open esp32_book_reader.ino in the Arduino IDE
Verify your board and port are selected correctly
Click Upload


💡 If the upload gets stuck on "Connecting...", hold the BOOT button on your ESP32 until uploading begins.


Usage
ActionControlScroll through book listPush joystick Up / DownOpen selected bookClick joystick buttonNext pagePush joystick RightPrevious pagePush joystick LeftSave bookmark & exitHold joystick button for 5 seconds
Bookmarks are saved to the ESP32's internal flash memory and automatically resumed the next time you open a book.

Troubleshooting
LCD shows nothing
The I2C address may not be 0x27. Run an I2C scanner sketch to find your display's address, then update this line in the code:
cpp#define LCD_ADDR 0x27  // Change this to match your display
SD Card fails to mount
Try lowering the SPI clock speed in SD.begin():
cpp// Change this:
SD.begin(SD_CS, SPI, 16000000);

// To this:
SD.begin(SD_CS, SPI, 4000000);
Compile error: No such file or directory

LiquidCrystal_I2C.h — Reinstall the library, ensuring you select Frank de Brabander's version
Preferences.h — This is ESP32-exclusive; reinstall the ESP32 board package from Board Manager

Joystick not responding

Double-check wiring — GPIOs 34/35 have no internal pull-up resistors
Verify the joystick VCC is connected to 3.3V, not 5V

![image alt](https://github.com/Fin-420/E-reader/blob/fa6019082138e60ece43af22008bfaee2eb92df8/IMG_1160%20(1).webp)


