# Gesture Controlled ESP32 Car (MPU6500 + ESP-NOW)

A wireless gesture-controlled robotic car built using two ESP32 boards, an MPU6500 accelerometer, and an L298N motor driver.  
The transmitter reads hand tilt and sends direction commands to the receiver using ESP-NOW (no WiFi required).

------------------------------------------------------------

## Features

- Gesture control: forward, backward, left, right  
- Fast, low-latency ESP-NOW communication  
- Single battery powers the car (ESP32 + L298N + motors)  
- 4-motor BO chassis  
- Adjustable motor speed using PWM  
- Clean and simple code structure  

------------------------------------------------------------

## Hardware Used

- 2 × ESP32 Dev Board  
- MPU6500 / MPU6050 Accelerometer  
- L298N Motor Driver  
- 4 × BO Motors  
- 7.4–12V Battery  
- Car chassis  
- Jumper wires  

------------------------------------------------------------

# Connections

## 1. Transmitter (ESP32 + MPU6500)

MPU6500 VCC → ESP32 3.3V
MPU6500 GND → ESP32 GND
MPU6500 SDA → ESP32 GPIO 21
MPU6500 SCL → ESP32 GPIO 22
MPU6500 AD0 → GND

Transmitter ESP32 powered via USB or LiPo.

shell
Copy code

------------------------------------------------------------

## 2. Receiver (ESP32 + L298N + Motors)

### ESP32 to L298N (control pins)
IN1 → GPIO 23
IN2 → GPIO 22
IN3 → GPIO 19
IN4 → GPIO 18
ENA → GPIO 25
ENB → GPIO 26

scss
Copy code

### Motor wiring (4 BO motors)
Left motors (M1, M2):
Left motors + → L298N OUT1
Left motors - → L298N OUT2

scss
Copy code

Right motors (M3, M4):
Right motors + → L298N OUT3
Right motors - → L298N OUT4

shell
Copy code

### Power wiring (single battery)
Battery + → L298N +12V
Battery - → L298N GND
L298N 5V → ESP32 5V
L298N GND → ESP32 GND (common ground required)

markdown
Copy code

Important:
- Do not connect L298N 5V to ESP32 3.3V.  
- ESP32 must share ground with L298N.

------------------------------------------------------------

## 3. ESP-NOW MAC Pairing

The receiver ESP32 prints its MAC address in Serial Monitor:
CAR ESP MAC: xx:xx:xx:xx:xx:xx

css
Copy code

Use this MAC in the transmitter code:
uint8_t receiverMac[] = { 0x__, 0x__, 0x__, 0x__, 0x__, 0x__ };

markdown
Copy code

------------------------------------------------------------

# How It Works

1. The transmitter reads the MPU6500 accelerometer (AX, AY).  
2. Tilting the hand generates:
   - F = Forward  
   - B = Backward  
   - L = Left  
   - R = Right  
   - S = Stop  
3. Commands are sent via ESP-NOW to the car ESP32.  
4. The receiver drives the motors using the L298N motor driver.  

------------------------------------------------------------

# Code Structure

/gesture_transmitter
transmitter.ino (reads MPU, sends ESP-NOW packets)

/gesture_receiver
receiver.ino (receives commands, drives motors)

markdown
Copy code

------------------------------------------------------------

# Power Notes

- The entire car runs from a single battery connected to L298N +12V.  
- L298N 5V regulator powers the ESP32 via the 5V pin.  
- Transmitter ESP32 is powered separately (USB/power bank).  

------------------------------------------------------------

# License
MIT License
