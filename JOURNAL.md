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

> What did I do?

At the start, I did some researching on how to build a fpv drone with custom flight controller, like the brand or specs of camera (analog or digital), ESC, motor, 
and other sensors. I plan to use bare ICs for my custom flight controller instead of modules. I've spend quite some time figuring out how to add symbol and footprint from lcsc to kicad using the lcsc library so that I wont have to spend time making custom symbol and footprints. Later, I spent a lot of time finding my esp32s3 devkit which i will be using as the main microcontroller for this project, which at last I downloaded the symbol and footprint library from snapmagic. Later that I also try to figure out what are the types of gnd (mainly analog gnd and power gnd), which later I also found out that I dont have to be so particular on separating the gnds, instead the placement of my components matter more when it comes to separating gnds (this is a more traditional way to design pcbs).

> Lapse 
- [31 minutes](https://lapse.hackclub.com/timelapse/4Rj7IFrFs96F) 
- [3 hours 14 minutes](https://lapse.hackclub.com/timelapse/sZxCJMg_yL6b)
- (rest of the hours are recorded inside stardance)

**Total time spent: 9 hours**


# 19 August 2026: Schematics part 1

Today I've been working on wiring up entire thing. For this schematics I rely heavily on the manufacturer's datasheet to wire everything up, in the midst of wiring up everything I've did some research for my case. Here's what I found out:
- ESP32-s3 have 4 strapping pins (GPIO 0,3,45,46), and I am gonna avoid using these pins unless I really have to
- There are two interrupt pins on the imu, and I only have to wire it one to my gpio pin
- For the imu, there are multiples protocol including I3C, I2C, spi and so on, im going to use spi for my custom flight controller as I think ill have enough pins available on the esp32s3 devkit
- The gps module requires a RF amplifier as im using an external chip antenna 
- The purpose of 1PPS pin on the gps module (for timing synchronization)
- External coin cell for my gps module for backup voltage
- So far I'm using XT60 connector for power transmission from the 4s Lipo battery, but this might change again eventually as I intend to connect the battery directly to the ESC, and the ESC will provide battery to the FC itself
- Placement of components matters more than separating ground types
- I might change the SDMMC mode of the MICRO SD card, probably from 4-bit down to 1-bit if more pins are required from the esp32-s3
- I've also did some digging on firmware side to check the ESP-FC repo has any wiring constrictions for my components
- Towards the half of this session I kinda got worried of firmware, so I went ahead to vs code and try to source of available firmware for me to use. But my rational quickly pulled me back to my priority which is schematics 

<img width="1058" height="723" alt="image" src="https://github.com/user-attachments/assets/c942bcbe-bff4-445e-b682-6b0e68ea9352" />

> Lapse
- [2 hours 42 minutes](https://lapse.hackclub.com/timelapse/RQaOAsdWm3QB)
- [2 hours 11 minutes](https://lapse.hackclub.com/timelapse/Dun-Pq8h2ed-)

**Total time spent: 4 hours 53 minutes**

# 20 August 2026: Schematics part 2

Today I had finally finished the schematics part for the flight controller itself. I've did a lot of research on the way to here ending up finishing the whole wirings. 
I've added a few things from previous session: 
- Jst connectors to connect the VTX and ESC
- MX1.25 connectors for the camera pins
- not planning to wiring up int2 pin on the IMU to save gpio space
- OSD IC to burn telemetry data to the video data before transferring video data to the VTX module
- Merging SPI connections for OSD IC and barometer (Im afraid of merging the IMU's SPI bus due to latency jittering)
- I plan to wire the ELRS receiver through soldering wires, so I have to assign dedicated pads on the FC later on designing the PCB
- Going to use a RX5808 for video receiver on the goggles
- I also planned to add a buzzer so I can create those drone impression sound effects when I power up the drone, but I dont have any more available pin spaces on the esp32s3.

On context, I will be removing the ESP32-s3 devkit from displaying onto the PCB, as I planned to connect the ESP32-S3 externally through the female pin connectors as shown below. 
<img width="1015" height="678" alt="image" src="https://github.com/user-attachments/assets/217109b9-f8f5-4dc1-9285-22564a9f0706" />

Everything looks kinda messy now. 

<img width="750" height="1700" alt="image" src="https://github.com/user-attachments/assets/3bac61a4-b06b-4102-9326-2b9db9bdb766" />

I thought that the camera's connector pin are using jst but until I reconfirmed with google, but in fact, founded out that instead of jst it's using molex picoblade connector.

> Research

I've did some research on these few topics on the way:
- Isit safe to merge different sensors to the same SPI busses. Yes so i had a shared SPI bus for most of my components (I tried to keep this off from high speed sensors like barometer and IMU)
- Types of modes for MICRO SD card connection, in the end used 4-bit mode SDMMC and founded out this mode is the fastest and reliable then goes, 1-bit mode and SPI
- Connections of VSYNC, HSYNC, and LOS pins of the OSD IC are mandatory or not in my case, also understood their functions of those pins

> Lapse
- [3 hours 33 minutes](https://lapse.hackclub.com/timelapse/XmuVitqd2Qe3)
- [4 hours 17 minutes](https://lapse.hackclub.com/timelapse/QyjttQfzMO-R)

**Total time spent: 7 hours 40 minutes**

# 2 October 2026: Cleaning up & adding new parts
From previous journal, my schematics are kinda messy as most of the components' symbol are scattered around the canvas. So, I managed to clean it up with lines and labels. I've also connected all the pins of all components to the socket pin symbol, according to the ESP32s3 devkit pinout, and it looks something like this.
<img width="1050" height="722" alt="Screenshot 2026-09-01 215916" src="https://github.com/user-attachments/assets/35694848-21bf-41f4-9480-582d86ee5ced" />
Other than this, my OCD mind keeps on wanting things to be perfect for this custom flight controller, and eventually I decided to remove some stuff and add in some new functions and components to replace those freed up pins from my MCU. 

**Here are the stuffs that are new over here:**
- I changed the variant of my MCU which is from a ESP32S3 devkitC to a ESP32S3 camera, the most distinctive difference over here is that the ESP32S3 camera have 20 pins while the ESP32S3 devkitC have 24 pins.
- I've already removed blackbox logging using Micro SD card as I will be fully dependent on the flash memory of my ESP32S3 camera itself, although it's just 13-14MB size of flash memory for storing.
- I've also added a SMD buzzer so I can recreate those commonly heard drone buzzer sound when the drone itself power up, with this function I could also used it as a rescue item, e.g. when the drone got disarmed during flight or any flight error will occur the the buzzer will make some tuned sound according to the situation.
- I've also replaced the ELRS receiver with a LORA module and a RF processor (namely a MCU, ESP8266, so it can communicate with the LORA module itself). Now, things will be more technical than ever as Im now messing with RFs. And doing so, I could also save up some cost inside the BOM.
- I've also removed a LDO regulator (5V to 3.3V) cuz im going to use the on-board LDO in the MCU, whereby im going to input 5V into the MCU and 3.3V will be output to all components that are using 3.3V

<img width="1350" height="808" alt="Screenshot 2026-09-30 144647" src="https://github.com/user-attachments/assets/eeb77602-99ed-4630-a4ab-bcd27db38317" />

This will be the final schematics, and next session I will soon proceed with managing footprints first then going into layout-ing. 

> Lapse
- [2 hours 27 minutes](https://lapse.hackclub.com/timelapse/gGVmfJon4BNI)
- [35 minutes](https://lapse.hackclub.com/timelapse/uKweIGeeuaxO)
- [1 hour 7 minutes](https://lapse.hackclub.com/timelapse/-a5yFwwTULqH)
- [2 hours 35 minutes](https://lapse.hackclub.com/timelapse/MgX_zaedv4po)
- [10 minutes](https://lapse.hackclub.com/timelapse/aBydVnGwjJaT)
- [11 minutes](https://lapse.hackclub.com/timelapse/LZ87xXXs8FVr)

**Total time spent: 7 hours 5 minutes**

# 3 October 2026: Assigning footprints
Today I have finished assigning all the footprints to the respective components. Most of my components like SMD chip inductor, resistor and capacitor will mainly be in 0603 packaging as I feel like it is the right size for my situation as I really need to place everything in a tight space with the smaller components I could possibly use and will be manageable when soldering even though it would be my first ever time soldering such small components. (ofc im going to use a hotgun or a heatplate for this). While rest of the "important" components like the sensors, ICs, crystals, jst connectors and others that are imported using the easyeda library already heave pre-assigned footprint. This actually saves me quite a lot of time. In the middle of the lapse session, I've also searched up whether my selected 22uH inductor from Coilank is suitable for my buck converter (TPS5430DDAR). Although, most of the capacitors, inductors and resistor are in 0603 packaging, there's some special component like 220uF and 47uF im unable to select 0603 as 220uF is a polarized bulk capacitor, so i have to use those aluminum ones, and for 47uF capacitor, im unable to find suitable 0603 packaging capacitor cuz its too expensive and out of stocked. During assigning the footprints to the respective components, I've also searched up each components that im going to purchase from lcsc, so I wont get wrong with the packaging. In contrast, if i first assigning them the packaging footprint I wanted, that might be some uncertainty mistakes im going to make as that package component might run out of stock or to expensive. 

<img width="1172" height="782" alt="Screenshot 2026-10-03 234548" src="https://github.com/user-attachments/assets/098e7562-9c4e-4e20-b07f-4f0f46baa9c4" />

> Lapse
- [2 hours 12 minutes](https://lapse.hackclub.com/timelapse/Lqp6Ees-28a3)

**Total time spent: 2 hours 12 minutes**
