---
TITLE: FPV drone
AUTHOR: EAN 
DESCRIPTION:
DATE CREATED: 25 july 2026
---

# 17 August 2026: Sourcing parts & Adding custom parts
> Sourcing parts

Over the past few weeks, I had been searching for the most cheapest and compatible parts for my first ever FPV drone. 
The reason it takes this long for me to do research and choosing the most suitable parts is because that FPV drones must equip a decent camera for cinematic medias. 
All in all, I decided to make a custom flight controller pcb with esp32-s3 as this esp32 have tons of gpio pins to connect to the drone's peripherals and sensors. 
> Camera
- picked analog camera. Digital Camera are expensive.
- Though it will look like an old tv resolution with bit and bots
> Video transmitter, ESC, RC receiver
- Im using open-sourced and manufactured products from aliexpress.
> Others
- The miscellaneous parts are from lcsc

**So far this is the latest BOM list**
<img width="1600" height="362" alt="image" src="https://github.com/user-attachments/assets/437e1e3b-76f4-45b6-97c6-576cf937637f" />

> Adding custom parts

I also added 2D models from lcsc right after I secured what parts I'm going to use in this project for the drone.
To achieve this, I used this https://github.com/uPesy/easyeda2kicad.py.git for exporting footprints, symbol and even 3D models of the parts. This way, I can save up tons of time cause I dont need to spend time on making custom symbols and footprints. 

**Total time spent: 9 hours**


# 19 August 2026: Schematics part 1

Today I've been working on wiring up entire thing. For this schematics I rely heavily on the manufacturer's datasheet to wire everything up, in the midst of wiring up everything I've did some research for my case. Here's what I found out:
- ESP32-s3 have 4 strapping pins (GPIO 0,3,45,46), and I am gonna avoid using these pins unless I really have to
- There are two interrupt pins on the imu, and I only have to wire it one to my gpio pin
- The gps module requires a RF amplifier as im using an external chip antenna 
- The purpose of 1PPS pin on the gps module (for timing synchronization)
- External coin cell for my gps module for backup voltage
- So far I'm using XT60 connector for power transmission from the 4s Lipo battery, but this might change again eventually as I intend to connect the battery directly to the ESC, and the ESC will provide battery to the FC itself
- Placement of components matters more than separating ground types
- I might change the SDMMC mode of the MICRO SD card, probably from 4-bit down to 1-bit if more pins are required from the esp32-s3

**Total time spent: 3 hours**
