# Raf --- ESP32 Virtual Pet with OLED Display and Web Control Panel

## Overview

Raf is an ESP32-based virtual pet system that combines an OLED animated
character, touch interaction, persistent stat tracking, and a
mobile-friendly web control panel hosted directly by the ESP32 device.

The pet begins as an egg, can be hatched through interaction, given a
custom name, and then maintained over time through both physical
interaction and a browser interface.

## Features

-   OLED animated pet displayed on an SSD1306 screen
-   Egg hatching system with interaction-based incubation
-   Custom pet naming
-   Touch input interaction
-   Wi‑Fi Access Point mode hosted by the ESP32
-   Embedded web dashboard with:
    -   factor bars
    -   overall health display
    -   feed and clean actions
    -   notification messages
-   Persistent storage using EEPROM
-   Automatic stat decay over time
-   Idle and blinking animations
-   Responsive web interface compatible with mobile devices

## Project Concept

The project implements a modern Tamagotchi-style system built using
ESP32 hardware.

The pet maintains four core factors:

-   Hunger
-   Petting
-   Attention
-   Cleanliness

These values gradually decrease over time. The user maintains the pet
by:

-   physically interacting with the touch sensor
-   feeding the pet through the browser interface
-   cleaning the pet through the browser interface

Before the pet is born, the device displays an egg animation. After
enough interaction events occur, the egg hatches and the user assigns
the pet a name.

## System Operation

### Egg Phase

During the initial state:

-   the OLED displays an egg animation
-   the web interface presents a hatching interface
-   the egg must be clicked ten times
-   once completed, the user enters a name
-   the pet is marked as hatched and stored in EEPROM

### Pet Phase

After hatching:

-   the OLED displays the animated pet
-   the web dashboard becomes active
-   the pet's factors are tracked and saved
-   blinking and idle animations run automatically
-   touch interaction increases affection
-   feeding and cleaning are performed through the browser

### Persistence

The following data are stored in EEPROM:

-   factor values
-   pet name
-   hatch state

This allows the system to preserve the pet state after restarts.

## Hardware Requirements

-   ESP32 microcontroller
-   SSD1306 OLED display (128x64 I2C)
-   Touch sensor or touch input pin
-   Jumper wires
-   USB or external power supply

## Pin Configuration

Defined in the firmware:

#define SDA_PIN 21 #define SCL_PIN 10 #define TOUCH_PIN 7

Pins used:

-   SDA: GPIO 21
-   SCL: GPIO 10
-   Touch input: GPIO 7

Verify that the chosen ESP32 board supports these pins.

## Libraries

The project uses the following libraries:

-   Wire
-   Adafruit GFX
-   Adafruit SSD1306
-   WiFi
-   WebServer
-   EEPROM
-   time

Install Adafruit GFX and Adafruit SSD1306 through the Arduino Library
Manager.

## Installation

1.  Clone the repository

git clone https://github.com/yourusername/raf-virtual-pet.git

2.  Open the project in Arduino IDE

3.  Install required libraries through the Library Manager

4.  Select the correct ESP32 board

5.  Upload the firmware to the ESP32

6.  Open the Serial Monitor at 115200 baud to view logs

## Wi‑Fi Operation

The ESP32 runs in Access Point mode.

Network name:

Raf

To connect:

1.  Open Wi‑Fi settings on a phone or computer
2.  Connect to the Raf network
3.  Open a web browser
4.  Navigate to:

http://192.168.4.1

The control dashboard will appear.

## HTTP Endpoints

/\
Displays the main interface.

/eggClick\
Registers an egg interaction event.

/saveName?name=Raf\
Stores the chosen pet name and completes hatching.

/status\
Returns current factor values in JSON format.

/feed\
Triggers feeding animation and increases hunger value.

/clean\
Triggers cleaning animation and increases cleanliness value.

## Gameplay Logic

### Factors

The pet maintains four factors:

typedef struct Factors { unsigned int hunger, petting, attention, clean;
};

### Overall Condition

The overall condition is calculated as the average of all factors.

### Factor Decay

Factors decrease over time according to configured rates.

Example configuration:

-   Hunger decreases to 80 percent
-   Petting decreases to 90 percent
-   Cleanliness decreases to 95 percent
-   Attention decreases to 70 percent

This simulates neglect if the pet is not maintained.

## Interaction Model

### Physical Interaction

Touching the sensor:

-   triggers petting animation
-   increases petting factor
-   slightly increases attention

### Web Interaction

Browser actions allow:

-   feeding the pet
-   cleaning the pet

### Emotional State

If overall health becomes low, the OLED animation switches to tired
expressions.

## Animations

The firmware includes multiple bitmap animations such as:

-   idle face
-   blinking
-   tired face
-   eating animation
-   shower animation
-   petting animation
-   egg states

All graphics are stored in program memory using PROGMEM.

## EEPROM Storage

EEPROM stores:

-   factor values
-   pet configuration
-   hatch state

Signature values are used to validate stored memory blocks.

## Project Architecture

The firmware contains several logical subsystems.

### Panel

Responsible for generating HTML content and rendering the web dashboard.

### Animations

Handles OLED bitmap playback and interaction animations.

### EEPROM Management

Handles saving and loading persistent data.

### Web Server

Handles browser requests and JSON responses.

### Main Loop

Handles:

-   animation timing
-   stat decay
-   touch input
-   HTTP request handling

## Security Note

If Wi‑Fi credentials exist in the firmware source code, they should not
be committed publicly. Replace them with placeholders or remove them if
unused.

## Possible Improvements

Potential future extensions:

-   additional actions such as sleep or play
-   sound feedback
-   battery-powered standalone version
-   multiple pet profiles
-   evolution system
-   mood-based expressions
-   OTA firmware updates
-   modular code structure using multiple files

## Summary

Raf is an embedded virtual pet platform combining hardware interaction,
animation, persistent storage, and a browser-based user interface. The
project demonstrates embedded web systems, OLED graphics programming,
and interactive device design using the ESP32 platform.
