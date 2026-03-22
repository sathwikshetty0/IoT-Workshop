# 🌐 IoT Workshop — ESP32 + Wokwi + ThingSpeak

> **Build real IoT systems from scratch — sensors, networking, cloud dashboards, and a working hackathon project.**

![ESP32](https://img.shields.io/badge/Board-ESP32-00D4AA?style=for-the-badge)
![Platform](https://img.shields.io/badge/Simulator-Wokwi-58A6FF?style=for-the-badge)
![Cloud](https://img.shields.io/badge/Cloud-ThingSpeak-FF6B35?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## 📋 Table of Contents

- [What You'll Build](#-what-youll-build)
- [Prerequisites](#-prerequisites)
- [Workshop Schedule](#-workshop-schedule)
- [Session 1 — IoT Basics & ESP32 Setup](#session-1--iot-basics--esp32-setup)
- [Session 2 — GPIO & PWM](#session-2--gpio--pwm)
- [Session 3 — Sensor Playground](#session-3--sensor-playground)
  - [Sensor 1 — DHT11 Temperature & Humidity](#sensor-1--dht11-temperature--humidity)
  - [Sensor 2 — LDR Light Sensor](#sensor-2--ldr-light-sensor)
  - [Sensor 3 — HC-SR04 Ultrasonic Distance](#sensor-3--hc-sr04-ultrasonic-distance)
  - [Sensor 4 — PIR Motion Sensor](#sensor-4--pir-motion-sensor)
  - [Sensor 5 — Potentiometer](#sensor-5--potentiometer)
- [Session 4 — Networking + ThingSpeak Cloud](#session-4--networking--thingspeak-cloud)
- [Session 5 — Data Visualization](#session-5--data-visualization)
- [Session 6 — Mini Hackathon Projects](#session-6--mini-hackathon-projects)
  - [Project A — Smart Room Automation](#project-a--smart-room-automation)
  - [Project B — Smart Parking System](#project-b--smart-parking-system)
  - [Project C — Smart Weather Station](#project-c--smart-weather-station)
  - [Project D — Smart Security System](#project-d--smart-security-system)
  - [Project E — Assistive Smart Helmet ⭐](#project-e--assistive-smart-helmet-)
- [ThingSpeak Setup Guide](#-thingspeak-setup-guide)
- [Common Errors & Fixes](#-common-errors--fixes)
- [Hardware Shopping List](#-hardware-shopping-list)
- [Resources & Next Steps](#-resources--next-steps)
- [Contributing](#-contributing)

---

## 🏗️ What You'll Build

By the end of this workshop you will have built:

| Project | Sensors Used | Cloud |
|---|---|---|
| LED Blink & PWM Fade | GPIO | — |
| Button-controlled LED | GPIO + Button | — |
| Temperature Alert System | DHT11 | Serial Monitor |
| Light-activated Lamp | LDR | — |
| Ultrasonic Distance Meter | HC-SR04 | — |
| Motion Detector | PIR | — |
| Analog LED Dimmer | Potentiometer | — |
| **Live IoT Dashboard** | DHT11 | **ThingSpeak** |
| **Complete Hackathon Project** | 2+ Sensors | **ThingSpeak** |

The full IoT pipeline you'll implement:

```
Sensor → ESP32 → WiFi → ThingSpeak Cloud → Live Dashboard → Alert
```

---

## ✅ Prerequisites

### Software (all free, no install needed)

| Tool | Link | What For |
|---|---|---|
| Wokwi Simulator | [wokwi.com](https://wokwi.com) | Run ESP32 code in browser — no hardware needed |
| ThingSpeak | [thingspeak.com](https://thingspeak.com) | Free IoT cloud dashboard |
| Arduino IDE *(optional)* | [arduino.cc/downloads](https://www.arduino.cc/en/software) | For real hardware later |

### Knowledge Required
- Basic programming concepts (variables, loops, if-else)
- No prior hardware or electronics experience needed

### For Real Hardware *(optional — not needed for workshop)*
- ESP32 DevKit board (~₹250)
- USB-A to Micro-USB cable
- Breadboard + jumper wires

---

## 📅 Workshop Schedule

```
9:30 AM  ──── Kickoff + IoT Mindset
10:00 AM ──── ESP32 Deep Dive (GPIO & PWM)
11:00 AM ──── ☕ Break
11:15 AM ──── 🔥 Sensor Playground (5 sensors)
12:45 PM ──── 🍽️ Lunch
1:30 PM  ──── Networking + ThingSpeak Cloud
2:30 PM  ──── Data Visualization + Insights
3:15 PM  ──── 🚀 Mini Hackathon
4:15 PM  ──── Demo + Wrap-Up
4:30 PM  ──── Done!
```

---

---

# Session 1 — IoT Basics & ESP32 Setup

## The IoT Pipeline

Every IoT system ever built follows this exact flow:

```
┌─────────┐    ┌─────────┐    ┌──────────┐    ┌─────────┐    ┌───────────┐    ┌─────────┐
│  SENSOR │───▶│  ESP32  │───▶│ INTERNET │───▶│  CLOUD  │───▶│ DASHBOARD │───▶│ ACTION  │
│  (Eyes) │    │ (Brain) │    │ (Highway)│    │(Memory) │    │  (Voice)  │    │(Result) │
└─────────┘    └─────────┘    └──────────┘    └─────────┘    └───────────┘    └─────────┘
```

## ESP32 vs ESP8266

| Feature | ESP8266 | ESP32 |
|---|---|---|
| Cores | 1 (single) | 2 (dual) |
| Clock Speed | 80 MHz | 240 MHz |
| RAM | 80 KB | 520 KB |
| Connectivity | WiFi only | WiFi + Bluetooth 4.2 + BLE |
| GPIO Pins | ~17 | 34+ |
| ADC Channels | 1 | Multiple |
| Cost | ~₹150 | ~₹250 |
| **Use this?** | ❌ Legacy | ✅ **Yes — we use this** |

## Opening Wokwi

1. Go to [wokwi.com](https://wokwi.com)
2. Click **Start from scratch**
3. Choose **ESP32**
4. You'll see the code editor on the left and the simulation on the right
5. Click the **▶ Play** button to run your code

---

---

# Session 2 — GPIO & PWM

## 💡 What is GPIO?

GPIO = **General Purpose Input Output**

Every pin on the ESP32 can be configured as:
- **INPUT** — reads data from the outside world (buttons, sensors)
- **OUTPUT** — sends voltage to control something (LEDs, relays, buzzers)

```
HIGH = 3.3V  |  LOW = 0V (GND)
```

## 💡 What is PWM?

PWM = **Pulse Width Modulation**

ESP32 is digital — it can only output `0V` or `3.3V`. PWM lets you **simulate analog output** by switching the pin on/off very fast (5000 times/second). The LED "sees" an average voltage.

```
Duty 0%   = always OFF  = 0 brightness
Duty 50%  = half ON/OFF = 50% brightness
Duty 100% = always ON   = full brightness
```

---

## Code 1 — LED Blink

**Wokwi components needed:** ESP32, LED, 220Ω resistor

```cpp
// LED Blink — the Hello World of hardware
// Connect: LED+ → Pin 2 → 220Ω resistor → GND

void setup() {
  pinMode(2, OUTPUT);  // set pin 2 as output
}

void loop() {
  digitalWrite(2, HIGH);  // LED ON
  delay(250);              // wait 250ms
  digitalWrite(2, LOW);   // LED OFF
  delay(250);              // wait 250ms
}
```

**Try it:** Change `250` to different values. What do you notice when it gets very small (like `50`)?

---

## Code 2 — Button Controls LED

**Wokwi components needed:** ESP32, LED, 220Ω resistor, Push Button

```cpp
// Button → LED control
// LED:    Pin 2  → 220Ω → GND
// Button: Pin 15 → GND (use INPUT_PULLUP)

#define LED_PIN    2
#define BUTTON_PIN 15

void setup() {
  pinMode(LED_PIN,    OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // internal pull-up resistor
}

void loop() {
  int buttonState = digitalRead(BUTTON_PIN);

  // INPUT_PULLUP: LOW = pressed, HIGH = released
  if (buttonState == LOW) {
    digitalWrite(LED_PIN, HIGH);  // button pressed → LED ON
  } else {
    digitalWrite(LED_PIN, LOW);   // button released → LED OFF
  }
}
```

**Try it:** Add `Serial.println("Button pressed!")` inside the if-block. Open the Serial Monitor to see it.

---

## Code 3 — PWM LED Auto-Fade

**Wokwi components needed:** ESP32, LED, 220Ω resistor

```cpp
// PWM Fade — LED breathes in and out
// Connect: LED+ → Pin 2 → 220Ω → GND

void setup() {
  ledcSetup(0, 5000, 8);   // channel 0, 5000Hz frequency, 8-bit resolution (0-255)
  ledcAttachPin(2, 0);      // attach pin 2 to channel 0
}

void loop() {
  // fade in
  for (int brightness = 0; brightness <= 255; brightness++) {
    ledcWrite(0, brightness);
    delay(8);
  }

  // fade out
  for (int brightness = 255; brightness >= 0; brightness--) {
    ledcWrite(0, brightness);
    delay(8);
  }
}
```

**How `ledcSetup` works:**
```
ledcSetup(channel, frequency, resolution)
  channel    = 0–15, a virtual PWM channel
  frequency  = Hz, how fast it pulses (5000 is good for LEDs)
  resolution = bits, 8-bit = values 0–255
```

**Challenge:** Change the `delay(8)` values to make the fade faster or slower. What happens at `delay(1)`?

---

---

# Session 3 — Sensor Playground

> **Rule of thumb:** Sensors = Inputs · ESP32 = Brain · Output = Decision

---

## Sensor 1 — DHT11 Temperature & Humidity

**Real-world use:** Smart ACs, weather stations, greenhouse monitoring, server room alerts

**Wokwi components:** ESP32, DHT11 sensor

### Wiring

```
DHT11 Pin 1 (VCC) → 3.3V
DHT11 Pin 2 (DATA)→ GPIO 4
DHT11 Pin 4 (GND) → GND
```

### Code — Basic Reading

```cpp
#include <DHT.h>

#define DHTPIN  4      // data pin
#define DHTTYPE DHT11  // sensor type

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
  Serial.println("DHT11 Ready!");
}

void loop() {
  float temperature = dht.readTemperature();  // Celsius
  float humidity    = dht.readHumidity();     // %

  // check for read errors
  if (isnan(temperature) || isnan(humidity)) {
    Serial.println("ERROR: Failed to read DHT11");
    return;
  }

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" °C");

  Serial.print("Humidity:    ");
  Serial.print(humidity);
  Serial.println(" %");

  Serial.println("---");
  delay(2000);  // DHT11 needs 1-2 sec between reads
}
```

### Code — With Alert Logic

```cpp
#include <DHT.h>

#define DHTPIN   4
#define DHTTYPE  DHT11
#define LED_PIN  2

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  dht.begin();
}

void loop() {
  float temp = dht.readTemperature();
  float hum  = dht.readHumidity();

  if (isnan(temp) || isnan(hum)) {
    Serial.println("Sensor read failed. Retrying...");
    delay(2000);
    return;
  }

  // Decision logic — this is where IoT intelligence lives
  if (temp > 30) {
    Serial.println("🔥 HEAT ALERT! Temperature too high!");
    digitalWrite(LED_PIN, HIGH);   // turn on warning LED
  } else if (hum > 70) {
    Serial.println("💧 HUMIDITY ALERT! Too humid!");
    digitalWrite(LED_PIN, HIGH);
  } else {
    Serial.println("✅ Environment OK");
    digitalWrite(LED_PIN, LOW);
  }

  Serial.print("Temp: "); Serial.print(temp); Serial.println("°C");
  Serial.print("Hum:  "); Serial.print(hum);  Serial.println("%");
  Serial.println("----------");

  delay(2000);
}
```

**Student challenges:**
- Change the temperature threshold. What value makes sense for your room?
- Add a third condition: if `temp < 15`, print `"Too cold!"`
- Count how many alerts fire in one minute

---

## Sensor 2 — LDR Light Sensor

**Real-world use:** Automatic street lights, phone screen brightness, camera auto-exposure

**Wokwi components:** ESP32, LDR (photoresistor), 10kΩ resistor

### How It Works

```
High light → Low resistance → High voltage at analog pin → High ADC value
Low light  → High resistance→ Low voltage at analog pin  → Low ADC value

ESP32 ADC range: 0 (dark) to 4095 (bright)
```

### Wiring (Voltage Divider)

```
3.3V → LDR → A0 pin → 10kΩ resistor → GND
```

### Code — Basic LDR Reading

```cpp
#define LDR_PIN A0  // analog pin
#define LED_PIN 2

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  int lightValue = analogRead(LDR_PIN);  // 0 = dark, 4095 = bright

  Serial.print("Light level: ");
  Serial.println(lightValue);

  // automatic light — LED turns on when dark
  if (lightValue < 500) {
    digitalWrite(LED_PIN, HIGH);  // dark → LED ON
    Serial.println("🌙 Dark — light ON");
  } else {
    digitalWrite(LED_PIN, LOW);   // bright → LED OFF
    Serial.println("☀️ Bright — light OFF");
  }

  delay(200);
}
```

### Code — LDR with Brightness Mapping

```cpp
// Map light level to LED brightness using PWM

#define LDR_PIN A0

void setup() {
  Serial.begin(9600);
  ledcSetup(0, 5000, 8);
  ledcAttachPin(2, 0);
}

void loop() {
  int lightValue = analogRead(LDR_PIN);  // 0–4095

  // invert: dark → bright LED, bright → dim LED
  int brightness = map(lightValue, 0, 4095, 255, 0);
  ledcWrite(0, brightness);

  Serial.print("LDR: "); Serial.print(lightValue);
  Serial.print(" | LED brightness: "); Serial.println(brightness);

  delay(100);
}
```

**Student challenges:**
- Find the threshold value for YOUR room by printing `lightValue` and covering the sensor
- Add a second condition: if `lightValue < 200`, blink the LED instead of just turning it on

---

## Sensor 3 — HC-SR04 Ultrasonic Distance

**Real-world use:** Car parking sensors, robot obstacle avoidance, liquid level measurement, people counting

**Wokwi components:** ESP32, HC-SR04 ultrasonic sensor

### How It Works

```
1. TRIG pin fires a 40kHz ultrasound pulse (10 microseconds)
2. Sound travels out, hits an object, bounces back
3. ECHO pin measures how long the echo takes to return
4. Distance = (time × speed of sound) / 2

Speed of sound = 343 m/s = 0.0343 cm/μs
Distance (cm)  = duration (μs) / 58
```

### Wiring

```
HC-SR04 VCC  → 5V (or 3.3V)
HC-SR04 TRIG → GPIO 5
HC-SR04 ECHO → GPIO 18
HC-SR04 GND  → GND
```

### Code — Basic Distance Measurement

```cpp
#define TRIG_PIN 5
#define ECHO_PIN 18

void setup() {
  Serial.begin(9600);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  Serial.println("Ultrasonic Distance Sensor Ready");
}

void loop() {
  // send 10 microsecond trigger pulse
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // measure echo return time (microseconds)
  long duration = pulseIn(ECHO_PIN, HIGH);

  // convert to centimetres
  long distance = duration / 58;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  delay(500);
}
```

### Code — Smart Parking Alert

```cpp
// Smart parking sensor — simulates car park occupancy detector

#define TRIG_PIN 5
#define ECHO_PIN 18
#define LED_PIN  2

#define OCCUPIED_THRESHOLD 10  // cm — closer than this = car present

void setup() {
  Serial.begin(9600);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LED_PIN,  OUTPUT);
}

long getDistance() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  long duration = pulseIn(ECHO_PIN, HIGH);
  return duration / 58;
}

void loop() {
  long distance = getDistance();

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.print(" cm — Spot: ");

  if (distance < OCCUPIED_THRESHOLD) {
    Serial.println("🔴 OCCUPIED");
    digitalWrite(LED_PIN, HIGH);
  } else {
    Serial.println("🟢 AVAILABLE");
    digitalWrite(LED_PIN, LOW);
  }

  delay(1000);
}
```

**Student challenges:**
- Add a counter — count how many times a spot was occupied in the last 10 readings
- Map distance 0–100 cm to LED brightness 255–0 (closer = brighter warning)

---

## Sensor 4 — PIR Motion Sensor

**Real-world use:** Security lights, burglar alarms, automatic doors, occupancy detection, hotel room lighting

**How it works:** PIR (Passive Infrared) detects **infrared radiation from warm moving objects** (like humans). It doesn't emit anything — it just listens. Output is digital: HIGH = motion detected, LOW = no motion.

**Wokwi components:** ESP32, PIR HC-SR501

### Wiring

```
PIR VCC    → 5V
PIR OUT    → GPIO 13
PIR GND    → GND
```

### Code — Basic Motion Detection

```cpp
#define PIR_PIN 13
#define LED_PIN 2

void setup() {
  Serial.begin(9600);
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  Serial.println("PIR sensor ready. Waiting for motion...");
  delay(2000);  // PIR needs ~2 seconds to calibrate on startup
}

void loop() {
  int motionState = digitalRead(PIR_PIN);

  if (motionState == HIGH) {
    Serial.println("⚠️ Motion detected!");
    digitalWrite(LED_PIN, HIGH);
    delay(3000);              // keep LED on for 3 seconds after motion
    digitalWrite(LED_PIN, LOW);
  }
}
```

### Code — Motion Counter with Timeout

```cpp
// Advanced: count motion events, add timeout, log with timestamp

#define PIR_PIN 13
#define LED_PIN 2

int motionCount   = 0;
bool motionActive = false;
unsigned long lastMotionTime = 0;
unsigned long TIMEOUT_MS     = 5000;  // reset after 5 seconds no motion

void setup() {
  Serial.begin(9600);
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  delay(2000);
  Serial.println("PIR Ready — monitoring...");
}

void loop() {
  bool motionNow = digitalRead(PIR_PIN);

  if (motionNow && !motionActive) {
    // new motion event started
    motionActive = true;
    lastMotionTime = millis();
    motionCount++;

    Serial.print("🚶 Motion event #");
    Serial.print(motionCount);
    Serial.print("  |  Time: ");
    Serial.print(millis() / 1000);
    Serial.println("s");

    digitalWrite(LED_PIN, HIGH);

    if (motionCount > 5) {
      Serial.println("🚨 HIGH TRAFFIC — More than 5 detections!");
    }
  }

  // check if motion has been gone long enough
  if (motionActive && !motionNow) {
    if (millis() - lastMotionTime > TIMEOUT_MS) {
      motionActive = false;
      digitalWrite(LED_PIN, LOW);
    }
  }

  delay(100);
}
```

**Student challenges:**
- Change `TIMEOUT_MS` to see how it affects LED behaviour
- Print the time between motion events to calculate average visit frequency

---

## Sensor 5 — Potentiometer

**Real-world use:** Volume knobs, brightness dimmers, servo position control, motor speed control

**How it works:** A potentiometer is a variable resistor. Turning the knob moves a wiper along a resistive track, changing the output voltage from 0V to 3.3V. ESP32 reads this as an analog value 0–4095.

**Wokwi components:** ESP32, Potentiometer

### Wiring

```
Pot left pin  → 3.3V
Pot middle pin→ GPIO 35 (analog input)
Pot right pin → GND
```

### Code — Potentiometer to LED Brightness

```cpp
// Rotate pot → control LED brightness via PWM

#define POT_PIN 35   // analog input
#define LED_PIN 2    // PWM output

void setup() {
  Serial.begin(9600);
  ledcSetup(0, 5000, 8);     // channel 0, 5kHz, 8-bit
  ledcAttachPin(LED_PIN, 0); // attach LED to channel 0
}

void loop() {
  int potValue  = analogRead(POT_PIN);           // 0–4095
  int dutyCycle = map(potValue, 0, 4095, 0, 255);// map to 0–255

  ledcWrite(0, dutyCycle);

  Serial.print("Pot: ");
  Serial.print(potValue);
  Serial.print("  |  Brightness: ");
  Serial.print(dutyCycle);
  Serial.print(" (");
  Serial.print(map(dutyCycle, 0, 255, 0, 100));
  Serial.println("%)");

  delay(50);
}
```

### Code — Potentiometer as Menu Selector

```cpp
// Pot controls which "mode" is active — 5 zones across the range

#define POT_PIN 35

void setup() {
  Serial.begin(9600);
}

void loop() {
  int val  = analogRead(POT_PIN);  // 0–4095
  int zone = map(val, 0, 4095, 1, 5);  // map to zones 1–5

  Serial.print("Value: "); Serial.print(val);
  Serial.print(" | Zone: "); Serial.print(zone);
  Serial.print(" | Mode: ");

  switch (zone) {
    case 1: Serial.println("🔵 Low Power Mode");   break;
    case 2: Serial.println("🟢 Eco Mode");         break;
    case 3: Serial.println("🟡 Normal Mode");      break;
    case 4: Serial.println("🟠 Performance Mode"); break;
    case 5: Serial.println("🔴 Max Power Mode");   break;
  }

  delay(200);
}
```

---

---

# Session 4 — Networking + ThingSpeak Cloud

## WiFi Connection

```cpp
#include <WiFi.h>

const char* ssid     = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

void setup() {
  Serial.begin(115200);

  WiFi.begin(ssid, password);
  Serial.print("Connecting");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\n✅ Connected!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());
}

void loop() {
  // your IoT code here
}
```

## HTTP vs MQTT

| Feature | HTTP | MQTT |
|---|---|---|
| Pattern | Request → Response | Publish / Subscribe |
| Overhead | High (headers, handshake) | Very low |
| Latency | Moderate | Near real-time |
| Best for | One-time data upload | Continuous streaming |
| Used by | ThingSpeak, REST APIs | Smart factories, Tesla, AWS IoT |
| Complexity | Simple | Moderate |

---

## 📡 ThingSpeak Setup Guide

### Step 1 — Create Account
1. Go to [thingspeak.com](https://thingspeak.com)
2. Click **Sign Up** → use any email → confirm

### Step 2 — Create a Channel
1. Click **Channels** → **New Channel**
2. Fill in:
   - **Name:** `ESP32 Weather Station`
   - **Field 1:** `Temperature`
   - **Field 2:** `Humidity`
3. Click **Save Channel**

### Step 3 — Get Your API Key
1. Open your channel → click **API Keys** tab
2. Copy the **Write API Key** (looks like: `ABCDEFGH12345678`)
3. ⚠️ Never share this key publicly — it lets anyone write to your channel

### Step 4 — Understanding the URL

The ThingSpeak upload URL is just a web address with parameters:

```
http://api.thingspeak.com/update
  ?api_key=YOUR_KEY_HERE
  &field1=27.5
  &field2=65.0
```

| Part | Meaning |
|---|---|
| `api.thingspeak.com/update` | ThingSpeak endpoint |
| `?api_key=` | Your authentication key |
| `&field1=` | Temperature value |
| `&field2=` | Humidity value |

### Response Codes

| Code | Meaning |
|---|---|
| `1`, `2`, `3`... | ✅ Success — entry number saved |
| `-1` | ❌ No internet / connection failed |
| `400` | ❌ Wrong API key or bad request |
| `0` | ❌ ThingSpeak rate limit hit |

---

## Code — DHT11 → ThingSpeak (Full Working Code)

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <DHT.h>

// ── WiFi Credentials ─────────────────────────────────────
const char* ssid     = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

// ── ThingSpeak ────────────────────────────────────────────
String apiKey = "YOUR_WRITE_API_KEY";  // paste your key here

// ── DHT11 ─────────────────────────────────────────────────
#define DHTPIN  4
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

// ── Setup ─────────────────────────────────────────────────
void setup() {
  Serial.begin(115200);
  dht.begin();

  // connect to WiFi
  Serial.print("Connecting to WiFi");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\n✅ WiFi Connected!");
  Serial.println("IP: " + WiFi.localIP().toString());
}

// ── Upload to ThingSpeak ───────────────────────────────────
void uploadData(float temperature, float humidity) {
  if (WiFi.status() != WL_CONNECTED) {
    Serial.println("❌ WiFi disconnected. Skipping upload.");
    return;
  }

  // build the URL
  String url = "http://api.thingspeak.com/update"
               "?api_key=" + apiKey +
               "&field1="  + String(temperature) +
               "&field2="  + String(humidity);

  HTTPClient http;
  http.begin(url);
  int responseCode = http.GET();
  http.end();

  if (responseCode > 0) {
    Serial.println("✅ Upload OK — Entry #" + String(responseCode));
  } else {
    Serial.println("❌ Upload failed. Code: " + String(responseCode));
  }
}

// ── Main Loop ─────────────────────────────────────────────
void loop() {
  float temp = dht.readTemperature();
  float hum  = dht.readHumidity();

  // validate readings
  if (isnan(temp) || isnan(hum)) {
    Serial.println("Sensor read failed. Retrying...");
    delay(2000);
    return;
  }

  Serial.println("=== Reading ===");
  Serial.print("Temp: "); Serial.print(temp); Serial.println("°C");
  Serial.print("Hum:  "); Serial.print(hum);  Serial.println("%");

  uploadData(temp, hum);

  // ThingSpeak free tier: max 1 update every 15 seconds
  // We use 20 seconds to be safe
  delay(20000);
}
```

---

---

# Session 5 — Data Visualization

## ThingSpeak Dashboard Features

| Feature | What it does | Where to find it |
|---|---|---|
| **Charts** | Auto-plotted live graph of each field | Channel → Private View |
| **Public View** | Shareable link — anyone can see your data | Channel → Public View |
| **React** | Trigger email/webhook when data crosses threshold | Apps → React |
| **ThingHTTP** | Make HTTP requests when data changes | Apps → ThingHTTP |
| **Data Export** | Download your history as CSV | Channel → Export |

## Setting a Temperature Alert (ThingSpeak React)

1. Go to **Apps → React**
2. Click **New React**
3. Set:
   - Condition: `Field 1 (Temperature) greater than 35`
   - Action: `ThingHTTP` or `Email notification`
4. Save

Now if your temperature goes above 35°C, ThingSpeak will automatically send an alert.

## Reading Your Data via API

```
# Get the last 10 readings as JSON
https://api.thingspeak.com/channels/YOUR_CHANNEL_ID/feeds.json?results=10

# Get only the last temperature value
https://api.thingspeak.com/channels/YOUR_CHANNEL_ID/fields/1/last.json
```

---

---

# Session 6 — Mini Hackathon Projects

> **Requirements for every project:**
> - Minimum 2 sensors
> - At least one `if/else` condition making a decision
> - Cloud simulation (real upload OR `Serial.println("Uploading...")`)

---

## Project A — Smart Room Automation

**Sensors:** LDR + PIR + LED  
**Concept:** Light only turns on when the room is dark AND someone is present. Saves energy.

### Wiring

```
LDR:     Analog pin A0, 10kΩ to GND
PIR:     GPIO 13
LED:     GPIO 2, 220Ω to GND
```

### Code

```cpp
#include <WiFi.h>
#include <HTTPClient.h>

#define LDR_PIN A0
#define PIR_PIN 13
#define LED_PIN 2

const char* ssid     = "YOUR_WIFI";
const char* password = "YOUR_PASS";
String apiKey        = "YOUR_THINGSPEAK_KEY";

// Threshold — adjust based on your room lighting
#define DARK_THRESHOLD 500

void setup() {
  Serial.begin(115200);
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) { delay(500); }
  Serial.println("WiFi Connected!");
}

void uploadStatus(int lightLevel, bool motion, bool lightOn) {
  String url = "http://api.thingspeak.com/update"
               "?api_key=" + apiKey +
               "&field1="  + String(lightLevel) +
               "&field2="  + String(motion ? 1 : 0) +
               "&field3="  + String(lightOn ? 1 : 0);
  HTTPClient http;
  http.begin(url);
  http.GET();
  http.end();
}

void loop() {
  int  lightLevel = analogRead(LDR_PIN);
  bool motion     = digitalRead(PIR_PIN);
  bool isDark     = lightLevel < DARK_THRESHOLD;

  bool lightShouldBeOn = isDark && motion;

  if (lightShouldBeOn) {
    digitalWrite(LED_PIN, HIGH);
    Serial.println("💡 Light ON — dark + motion detected");
  } else {
    digitalWrite(LED_PIN, LOW);
    if (!isDark)  Serial.println("🌞 Room bright — light OFF");
    if (!motion)  Serial.println("🚶 No motion — light OFF");
  }

  Serial.print("LDR: "); Serial.print(lightLevel);
  Serial.print(" | Motion: "); Serial.println(motion ? "YES" : "NO");

  uploadStatus(lightLevel, motion, lightShouldBeOn);
  delay(20000);
}
```

---

## Project B — Smart Parking System

**Sensors:** HC-SR04 Ultrasonic  
**Concept:** Detect if a parking spot is occupied. Display status. Log to cloud.

### Code

```cpp
#include <WiFi.h>
#include <HTTPClient.h>

#define TRIG_PIN 5
#define ECHO_PIN 18
#define RED_LED  2    // occupied
#define GREEN_LED 4   // available

const char* ssid     = "YOUR_WIFI";
const char* password = "YOUR_PASS";
String apiKey        = "YOUR_THINGSPEAK_KEY";

#define CAR_PRESENT_CM 15  // distance threshold in cm

long getDistance() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  return pulseIn(ECHO_PIN, HIGH) / 58;
}

void setup() {
  Serial.begin(115200);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(RED_LED,  OUTPUT);
  pinMode(GREEN_LED,OUTPUT);

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) { delay(500); }
  Serial.println("WiFi Connected!");
}

void loop() {
  long distance = getDistance();
  bool occupied = distance < CAR_PRESENT_CM;

  if (occupied) {
    digitalWrite(RED_LED,   HIGH);
    digitalWrite(GREEN_LED, LOW);
    Serial.println("🔴 SPOT OCCUPIED — distance: " + String(distance) + "cm");
  } else {
    digitalWrite(RED_LED,   LOW);
    digitalWrite(GREEN_LED, HIGH);
    Serial.println("🟢 SPOT AVAILABLE — distance: " + String(distance) + "cm");
  }

  // upload to ThingSpeak
  String url = "http://api.thingspeak.com/update"
               "?api_key=" + apiKey +
               "&field1="  + String(distance) +
               "&field2="  + String(occupied ? 1 : 0);

  HTTPClient http;
  http.begin(url);
  http.GET();
  http.end();

  delay(20000);
}
```

---

## Project C — Smart Weather Station

**Sensors:** DHT11  
**Concept:** Read temperature + humidity, upload every 20 seconds to ThingSpeak, view live graph.

*(Full code in [Session 4](#code--dht11--thingspeak-full-working-code))*

### Enhanced Version with Trend Detection

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <DHT.h>

#define DHTPIN  4
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

const char* ssid = "YOUR_WIFI";
const char* pass = "YOUR_PASS";
String apiKey    = "YOUR_KEY";

float prevTemp = 0;
float prevHum  = 0;

void setup() {
  Serial.begin(115200);
  dht.begin();
  WiFi.begin(ssid, pass);
  while (WiFi.status() != WL_CONNECTED) { delay(500); }
}

void loop() {
  float temp = dht.readTemperature();
  float hum  = dht.readHumidity();

  if (isnan(temp) || isnan(hum)) { delay(2000); return; }

  // trend detection
  if (prevTemp > 0) {
    float tempChange = temp - prevTemp;
    if (tempChange >  2.0) Serial.println("🔺 Temperature rising fast!");
    if (tempChange < -2.0) Serial.println("🔻 Temperature dropping fast!");
  }

  prevTemp = temp;
  prevHum  = hum;

  // alert system
  if (temp > 35)     Serial.println("🔥 HEAT ALERT");
  else if (temp < 15) Serial.println("🥶 COLD ALERT");
  if (hum > 80)      Serial.println("💧 HIGH HUMIDITY ALERT");

  String url = "http://api.thingspeak.com/update"
               "?api_key=" + apiKey +
               "&field1=" + String(temp) +
               "&field2=" + String(hum);

  HTTPClient http;
  http.begin(url);
  int code = http.GET();
  http.end();
  Serial.println("Upload: " + String(code));

  delay(20000);
}
```

---

## Project D — Smart Security System

**Sensors:** PIR + DHT11  
**Concept:** Motion triggers alert. Temperature monitors server room / storage area. All logged to cloud.

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <DHT.h>

#define PIR_PIN  13
#define LED_PIN  2
#define BUZZ_PIN 4    // optional buzzer
#define DHTPIN   15
#define DHTTYPE  DHT11

DHT dht(DHTPIN, DHTTYPE);

const char* ssid = "YOUR_WIFI";
const char* pass = "YOUR_PASS";
String apiKey    = "YOUR_KEY";

int alertCount = 0;

void setup() {
  Serial.begin(115200);
  pinMode(PIR_PIN,  INPUT);
  pinMode(LED_PIN,  OUTPUT);
  pinMode(BUZZ_PIN, OUTPUT);
  dht.begin();

  WiFi.begin(ssid, pass);
  while (WiFi.status() != WL_CONNECTED) { delay(500); }
  Serial.println("🔒 Security System ARMED");
  delay(2000);  // PIR calibration
}

void triggerAlert(String reason) {
  alertCount++;
  Serial.println("🚨 ALERT #" + String(alertCount) + " — " + reason);

  // visual + audio alert
  for (int i = 0; i < 3; i++) {
    digitalWrite(LED_PIN,  HIGH);
    digitalWrite(BUZZ_PIN, HIGH);
    delay(200);
    digitalWrite(LED_PIN,  LOW);
    digitalWrite(BUZZ_PIN, LOW);
    delay(200);
  }
}

void loop() {
  bool  motion = digitalRead(PIR_PIN);
  float temp   = dht.readTemperature();
  float hum    = dht.readHumidity();

  if (motion) triggerAlert("Motion detected");
  if (!isnan(temp) && temp > 40) triggerAlert("Overheating: " + String(temp) + "°C");

  // upload all data
  String url = "http://api.thingspeak.com/update"
               "?api_key=" + apiKey +
               "&field1="  + String(motion ? 1 : 0) +
               "&field2="  + String(isnan(temp) ? 0 : temp) +
               "&field3="  + String(alertCount);

  HTTPClient http;
  http.begin(url);
  http.GET();
  http.end();

  delay(20000);
}
```

---

## Project E — Assistive Smart Helmet ⭐

**Sensors:** DHT11 + PIR + Ultrasonic  
**Concept:** Safety helmet for construction workers. Monitors environment, detects proximity hazards, logs worker exposure data.

**This is the standout project. Build this if you want something portfolio-worthy.**

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <DHT.h>

// ── Pin Definitions ────────────────────────────────────────
#define DHTPIN     4
#define DHTTYPE    DHT11
#define PIR_PIN    13
#define TRIG_PIN   5
#define ECHO_PIN   18
#define LED_RED    2    // danger indicator
#define LED_GREEN  15   // all clear
#define BUZZER_PIN 16

// ── Thresholds ─────────────────────────────────────────────
#define TEMP_DANGER_C    38     // heat stress threshold
#define OBJECT_DANGER_CM 30     // proximity hazard threshold
#define HUM_DANGER_PCT   85     // high humidity (fog/steam)

// ── WiFi + ThingSpeak ──────────────────────────────────────
const char* ssid = "YOUR_WIFI";
const char* pass = "YOUR_PASS";
String apiKey    = "YOUR_KEY";

DHT dht(DHTPIN, DHTTYPE);

struct HelmetStatus {
  float temp, hum;
  long  distance;
  bool  motionNearby;
  bool  danger;
  String dangerReason;
};

long getDistance() {
  digitalWrite(TRIG_PIN, LOW);  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH); delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  return pulseIn(ECHO_PIN, HIGH) / 58;
}

void alertWorker(String reason) {
  Serial.println("⛑️ HELMET ALERT: " + reason);
  for (int i = 0; i < 2; i++) {
    digitalWrite(LED_RED,    HIGH);
    digitalWrite(BUZZER_PIN, HIGH);
    delay(300);
    digitalWrite(LED_RED,    LOW);
    digitalWrite(BUZZER_PIN, LOW);
    delay(150);
  }
}

HelmetStatus checkConditions() {
  HelmetStatus s;
  s.temp        = dht.readTemperature();
  s.hum         = dht.readHumidity();
  s.distance    = getDistance();
  s.motionNearby= digitalRead(PIR_PIN);
  s.danger      = false;
  s.dangerReason= "";

  if (!isnan(s.temp) && s.temp > TEMP_DANGER_C) {
    s.danger = true;
    s.dangerReason += "Heat stress (" + String(s.temp) + "°C). ";
  }
  if (s.distance > 0 && s.distance < OBJECT_DANGER_CM) {
    s.danger = true;
    s.dangerReason += "Object nearby (" + String(s.distance) + "cm). ";
  }
  if (!isnan(s.hum) && s.hum > HUM_DANGER_PCT) {
    s.danger = true;
    s.dangerReason += "High humidity (" + String(s.hum) + "%). ";
  }
  return s;
}

void setup() {
  Serial.begin(115200);
  dht.begin();
  pinMode(PIR_PIN,    INPUT);
  pinMode(TRIG_PIN,   OUTPUT);
  pinMode(ECHO_PIN,   INPUT);
  pinMode(LED_RED,    OUTPUT);
  pinMode(LED_GREEN,  OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);

  WiFi.begin(ssid, pass);
  while (WiFi.status() != WL_CONNECTED) { delay(500); }

  Serial.println("⛑️ Smart Helmet System ONLINE");
  digitalWrite(LED_GREEN, HIGH);
  delay(2000);
  digitalWrite(LED_GREEN, LOW);
}

void loop() {
  HelmetStatus status = checkConditions();

  if (status.danger) {
    alertWorker(status.dangerReason);
    digitalWrite(LED_RED,   HIGH);
    digitalWrite(LED_GREEN, LOW);
  } else {
    digitalWrite(LED_RED,   LOW);
    digitalWrite(LED_GREEN, HIGH);
    Serial.println("✅ All clear");
  }

  // log all readings
  Serial.println("Temp: " + String(status.temp) + "°C | Hum: " +
                 String(status.hum) + "% | Dist: " +
                 String(status.distance) + "cm");

  // upload to ThingSpeak
  String url = "http://api.thingspeak.com/update"
               "?api_key=" + apiKey +
               "&field1="  + String(isnan(status.temp) ? 0 : status.temp) +
               "&field2="  + String(isnan(status.hum)  ? 0 : status.hum) +
               "&field3="  + String(status.distance) +
               "&field4="  + String(status.danger ? 1 : 0);

  HTTPClient http;
  http.begin(url);
  http.GET();
  http.end();

  delay(20000);
}
```

---

---

# 🛠️ Common Errors & Fixes

| Error | Cause | Fix |
|---|---|---|
| LED doesn't blink | Wrong pin number | ESP32 built-in LED = pin 2. Check your Wokwi wiring. |
| DHT11 reads `NaN` | Wrong DHTPIN | Match the pin number to your Wokwi wiring exactly. |
| `isnan()` always true | Sensor not initialised | Make sure `dht.begin()` is in `setup()`. |
| WiFi never connects | Wrong SSID/password | Case-sensitive. Try copy-pasting, not retyping. |
| ThingSpeak returns `-1` | No internet connection | Use Wokwi's built-in WiFi or check your hotspot. |
| ThingSpeak returns `400` | Wrong API key | Re-copy from ThingSpeak API Keys tab. No extra spaces. |
| ThingSpeak returns `0` | Sending too fast | Increase `delay()` to at least `15000` (15 seconds). |
| Graph shows flat `0` | DHT read failing silently | Add `Serial.println(temp)` before the URL to debug. |
| Ultrasonic always `0 cm` | TRIG/ECHO pins swapped | Verify pin defines match your wiring exactly. |
| PIR never triggers | Warm-up not complete | Add `delay(2000)` at end of `setup()` for PIR calibration. |
| PWM LED always full brightness | `ledcSetup()` not called | Add `ledcSetup(0, 5000, 8)` before `ledcAttachPin()`. |
| Serial Monitor shows garbage | Baud rate mismatch | Match `Serial.begin(115200)` in code with monitor baud rate. |
| `HTTPClient` not found | Missing library | In Wokwi, ESP32 WiFi/HTTP is built-in. In Arduino IDE: install ESP32 board package. |

---

# 🛒 Hardware Shopping List

> *All prices approximate. These components are available on Robocraze, Robu.in, Amazon India.*

| Component | Price | Where Used |
|---|---|---|
| ESP32 DevKit v1 | ₹250 | Everything |
| DHT11 Temperature + Humidity | ₹30 | Sessions 3, 4, Projects C, D, E |
| HC-SR04 Ultrasonic | ₹50 | Session 3, Projects B, E |
| PIR HC-SR501 | ₹40 | Session 3, Projects D, E |
| LDR (photoresistor) | ₹10 | Session 3, Project A |
| Potentiometer 10kΩ | ₹15 | Session 3 |
| LEDs (pack of 10) | ₹20 | All sessions |
| 220Ω resistors (pack) | ₹20 | LED circuits |
| 10kΩ resistors (pack) | ₹20 | LDR divider |
| Push buttons (pack of 5) | ₹20 | Session 2 |
| Buzzer (active) | ₹20 | Projects D, E |
| Breadboard (full size) | ₹60 | Everything |
| Jumper wires (pack of 40) | ₹50 | Everything |
| USB-A to Micro-USB cable | ₹60 | Programming |
| **TOTAL** | **~₹615** | Full workshop on real hardware |

---

# 📚 Resources & Next Steps

### Documentation
- [ESP32 Arduino Reference](https://docs.espressif.com/projects/arduino-esp32/en/latest/)
- [Wokwi Documentation](https://docs.wokwi.com)
- [ThingSpeak Documentation](https://www.mathworks.com/help/thingspeak/)
- [DHT Library Reference](https://github.com/adafruit/DHT-sensor-library)

### Learning Resources
- [Random Nerd Tutorials](https://randomnerdtutorials.com) — Best ESP32 tutorials
- [Last Minute Engineers](https://lastminuteengineers.com) — Sensor guides with schematics
- [Wokwi Projects Gallery](https://wokwi.com/projects) — Community projects to learn from

### Next Projects to Build
| Project | New Skills |
|---|---|
| Smart door lock with RFID | SPI communication, RFID reader |
| Soil moisture monitor | Capacitive sensor, automated watering |
| GPS tracker | UART, NEO-6M GPS module |
| Energy meter | CT sensor, power calculation |
| OLED display dashboard | I2C, SSD1306 display library |
| Bluetooth LED controller | ESP32 BLE, mobile app integration |

### Cloud Platform Options

| Platform | Free Tier | Best For |
|---|---|---|
| **ThingSpeak** | 4 channels, 3M msgs/yr | Learning, prototyping |
| **Blynk** | Limited | Mobile dashboard apps |
| **Firebase** | 1 GB storage | Real-time JSON database |
| **AWS IoT Core** | 12 months free | Production systems |
| **Azure IoT Hub** | 8000 msgs/day | Enterprise IoT |
| **MQTT (Mosquitto)** | Self-host free | Real-time pub/sub |

---

# 🤝 Contributing

Found a bug? Have a better code example? Want to add a project?

1. Fork this repository
2. Create a branch: `git checkout -b my-improvement`
3. Commit: `git commit -m "Add: better ultrasonic example"`
4. Push: `git push origin my-improvement`
5. Open a Pull Request

---

## 📄 License

MIT License — free to use, modify, and distribute with attribution.

---

<div align="center">

**Built for the IoT Workshop — ESP32 + Wokwi + ThingSpeak**

*Sensors → ESP32 → Internet → Cloud → Dashboard → Action*

⭐ Star this repo if it helped you build something awesome

</div>
