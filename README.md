# CAR LOGO PROJECTOR HACK

![image](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624664003-4a0c4201-5f30-4d87-b328-ceb71bbe9f61.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T194638Z&X-Amz-Expires=300&X-Amz-Signature=13a989c7c93af35890152bf6c36578d014ded9366e8243a0c707d62837cbc7dc&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)

## From sketchy Hall-effect sensor to robust mechanical switch
There are very cool and cheap aftermarket car logo projectors which are installed on cars door. They give awesome image, but their activation mechanism can be a nightmare because they rely on a tiny magnet taped to the door sill, meant to trigger a Hall-effect sensor on the device.

**The problem is that magnet constantly gets kicked and ripped off by foot** when getting in and out of the car. This project solves that issue permanently by replacing the flaky Hall-effect sensor with a reliable mechanical pushbutton switch.

# Device
![Device](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624688064-9c341745-6dc5-4e69-81cb-86cb1b17730b.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T194852Z&X-Amz-Expires=300&X-Amz-Signature=135b5f4f11a07d575ed27340379303593723869dc3963d13d2b27134b87bb450&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)
![Sensor is marked](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624688635-c79b3b0f-aa66-4b00-90f3-feb8846713d1.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T194958Z&X-Amz-Expires=300&X-Amz-Signature=dc0ddab7ac94b8d513de25dfa9531676c55b12cdab463a1b91f6a08b3e1f77fb&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)

## How it works
The cycle begins by waking the central 8-pin microcontroller (MCU) from deep sleep with opening a door and setting input pin of the MCU to the logic HIGH state. Once awake, the MCU pulls the Gate of the A1SHB (Si2301) P-channel MOSFET down to ground (LOW), causing it to conduct and pass current to the main LED. The light from this high-power LED then passes through a miniature optical assembly—first through a translucent film printed with the logo, and then through a lens system that focuses and sharply projects that image onto the pavement. While the logo is illuminated, the MCU continuously monitors the output signal from the 1204 Hall-effect sensor. The moment the car door is closed and the device physically approaches the magnet taped to the door sill, the Hall-effect sensor detects the magnetic field and sets input pin of the MCU to the LOW state. The MCU then immediately sets the MOSFET Gate to a logic HIGH, breaking the circuit to the LED, turning off the light, and returning the entire system to a power-efficient sleep mode to save the battery. MCU also features timer, so main LED wont receive power after 2 mins. Next cycle begins after another HIGH signal from Hall-effect sensor, meaning door is open and image should be projetcted again.

I have extracted the schematics:
![Schematics](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624689066-76c2281d-a265-4ff1-a214-a778409c7a83.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195051Z&X-Amz-Expires=300&X-Amz-Signature=eaf7439c26380ee811d357266972bf7abb954ae2a374a3e2e7dd82bd30ce3f26&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)

# Hacking
We want to remove 1204 and put mechanical pushbutton on input pin.

Carefully desolder onyl sensor
![Sensor removed](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624689534-a639057f-c7f1-412b-8d7f-4ef756d171ed.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195131Z&X-Amz-Expires=300&X-Amz-Signature=149bcabf5851d3d6ab87fd6632d24e9c2e74609615ff7667d2438744e81bf309&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)


Then, we should solder 10k pull-up resistor between input pin and +3.7v
![pull-up added](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624690136-749d42ad-a78b-48e6-967b-51a7ef794cbc.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195242Z&X-Amz-Expires=300&X-Amz-Signature=0a99e3ee02acfb6daf0810e3ad7613d2f10be7bf3f91f1a4f73fd009e8c01cfa&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)
If we didn't use a pull-up resistor, input pin will be in float state (unstable) when pushbutton is not pressed. Also, scratch some soldermask from the input pin trace.

And then, solder new wires. One to the GND, and another to the input pin trace.
![new wires](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624690623-c54d7022-7af3-440b-81f7-65e6f837b0ee.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195324Z&X-Amz-Expires=300&X-Amz-Signature=d562781f1040bae7b8c1334324b513d0c00479a1a27f5e792a2d92eaec633a9e&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)

New wires get trough the back of the plastic case and solder ends to the new door pushbutton. Measure needed length first and try to get bigger pushbutton.
![alt text](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624690983-666ad433-318d-48f2-b6d8-c1674380d47d.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195402Z&X-Amz-Expires=300&X-Amz-Signature=c8736974b282b5874984d2823da79f34d1b178de239a7b4ee5025b182ac5bba3&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)

Then, install whole assembly onto your car door. Hide wires below and glue some coasters if needed.

![Glued](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624691301-65a1609b-2fa9-41f1-bec5-d460120dabac.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195428Z&X-Amz-Expires=300&X-Amz-Signature=5c454deabc6e4b9192a9e483e808655f588af5b10d57522c12139c37b14cb0a1&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)
![alt text](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624691564-8897c837-9970-46ce-8872-7928ed6d3f5e.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195455Z&X-Amz-Expires=300&X-Amz-Signature=9e9101f5d9c9b901a13ded72c94acf70be45dd1ef5e74be50000403424541561&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)

![alt text](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624691887-82297370-4c2c-44b0-bbfd-1af264ff9bdb.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195546Z&X-Amz-Expires=300&X-Amz-Signature=32ee6f744d6ee36fe2cbc89a9f610cc952ec78e292824c57330ffb49401b665f&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)
![final look](https://github-production-user-asset-6210df.s3.amazonaws.com/43916607/624692323-3df1e35d-323a-47af-9bcb-a35e6cc38e25.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260721%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260721T195624Z&X-Amz-Expires=300&X-Amz-Signature=fbc80c4c7d0b5ebc78485ab52e83b3d13b9ad0388b8feaed6288bae22a72dae3&X-Amz-SignedHeaders=host&response-content-type=image%2Fjpeg)

Now projector is controlled by the mechanical pushbutton, and there is no threat of ripping off parts while entering the vehicle.