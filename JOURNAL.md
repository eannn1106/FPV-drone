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
