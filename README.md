# CAR LOGO PROJECTOR HACK

<img width="2185" height="2230" alt="Projection" src="https://github.com/user-attachments/assets/2c6db522-12d2-4b68-862e-3dfc552b38f9" />

## From sketchy Hall-effect sensor to robust mechanical switch
There are very cool and cheap aftermarket car logo projectors which are installed on cars door. They give awesome image, but their activation mechanism can be a nightmare because they rely on a tiny magnet taped to the door sill, meant to trigger a Hall-effect sensor on the device.

**The problem is that magnet constantly gets kicked and ripped off by foot** when getting in and out of the car. This project solves that issue permanently by replacing the flaky Hall-effect sensor with a reliable mechanical pushbutton switch.

# Device
<img width="2296" height="2294" alt="Device" src="https://github.com/user-attachments/assets/8b60621d-d3b2-4cca-8ab3-6ced79de8c54" />
<img width="2376" height="1458" alt="Sensor is marked" src="https://github.com/user-attachments/assets/c0c9191b-e694-4d9d-a17d-3f217b077781" />

## How it works
The cycle begins by waking the central 8-pin microcontroller (MCU) from deep sleep with opening a door and setting input pin of the MCU to the logic HIGH state. Once awake, the MCU pulls the Gate of the A1SHB (Si2301) P-channel MOSFET down to ground (LOW), causing it to conduct and pass current to the main LED. The light from this high-power LED then passes through a miniature optical assembly—first through a translucent film printed with the logo, and then through a lens system that focuses and sharply projects that image onto the pavement. While the logo is illuminated, the MCU continuously monitors the output signal from the 1204 Hall-effect sensor. The moment the car door is closed and the device physically approaches the magnet taped to the door sill, the Hall-effect sensor detects the magnetic field and sets input pin of the MCU to the LOW state. The MCU then immediately sets the MOSFET Gate to a logic HIGH, breaking the circuit to the LED, turning off the light, and returning the entire system to a power-efficient sleep mode to save the battery. MCU also features timer, so main LED wont receive power after 2 mins. Next cycle begins after another HIGH signal from Hall-effect sensor, meaning door is open and image should be projetcted again.

I have extracted the schematics:
<img width="3833" height="1578" alt="Schematics" src="https://github.com/user-attachments/assets/8e0051fa-39e6-453b-aba3-2879712ab3a8" />

# Hacking
We want to remove 1204 and put mechanical pushbutton on input pin.

Carefully desolder onyl sensor
<img width="2296" height="980" alt="Sensor removed" src="https://github.com/user-attachments/assets/a0b400d1-367c-4451-b035-ea52eb369d36" />


Then, we should solder 10k pull-up resistor between input pin and +3.7v
<img width="2780" height="1112" alt="Pull-up added" src="https://github.com/user-attachments/assets/df3f1da3-b7d0-4a73-af36-3b5764aacfc2" />
If we didn't use a pull-up resistor, input pin will be in float state (unstable) when pushbutton is not pressed. Also, scratch some soldermask from the input pin trace.

And then, solder new wires. One to the GND, and another to the input pin trace.
<img width="2015" height="1043" alt="New wires" src="https://github.com/user-attachments/assets/85ad8d59-f03d-49ad-81ee-c983e5b038ac" />

New wires get trough the back of the plastic case and solder ends to the new door pushbutton. Measure needed length first and try to get bigger pushbutton.
<img width="2296" height="987" alt="Pushbutton" src="https://github.com/user-attachments/assets/10acc922-dc74-4222-af83-bc4b256deca4" />

Then, install whole assembly onto your car door. Hide wires below and glue some coasters if needed.

<img width="2296" height="2437" alt="Glued" src="https://github.com/user-attachments/assets/6ec58fa4-fe15-41a5-b03f-949962a82427" />
<img width="4080" height="2021" alt="Image" src="https://github.com/user-attachments/assets/7086594f-2e8a-4830-8dd8-7161d84fcbf6" />

<img width="2127" height="2648" alt="Image" src="https://github.com/user-attachments/assets/3ef1d659-ec89-416c-adfe-5580f445c8f9" />
<img width="3227" height="2229" alt="Image" src="https://github.com/user-attachments/assets/5f6a5679-10a6-4287-8df2-c8b5e9685ede" />

Now projector is controlled by the mechanical pushbutton, and there is no threat of ripping off parts while entering the vehicle.