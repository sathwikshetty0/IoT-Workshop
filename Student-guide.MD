# 📘 Student Guide — IoT Workshop
### ESP32 · Wokwi · ThingSpeak

> **Your complete self-study reference for everything covered in the workshop.**  
> All code, all steps, all sensor setups — in one place.

---

## 📋 Table of Contents

- [What is IoT?](#-what-is-iot)
- [The IoT Pipeline](#-the-iot-pipeline)
- [Getting Started with Wokwi](#-getting-started-with-wokwi)
- [Chapter 1 — ESP32 Basics](#-chapter-1--esp32-basics)
- [Chapter 2 — GPIO & PWM](#-chapter-2--gpio--pwm)
- [Chapter 3 — Sensors](#-chapter-3--sensors)
- [Chapter 4 — WiFi & ThingSpeak Cloud](#-chapter-4--wifi--thingspeak-cloud)
- [Chapter 5 — Build Your Project](#-chapter-5--build-your-project)
- [Quick Reference Card](#-quick-reference-card)
- [What to Learn Next](#-what-to-learn-next)

---

## 🌐 What is IoT?

**IoT = Internet of Things**

Any physical device that:
1. **Senses** something from the real world
2. **Processes** that data with a microcontroller
3. **Connects** to the internet
4. **Acts** on the result

### Real Examples You Use Every Day

| Device | Sensor | Action |
|---|---|---|
| Smart AC | Temperature sensor | Adjusts cooling automatically |
| Phone screen brightness | Light sensor (LDR) | Dims in dark, brightens outdoors |
| Car parking sensor | Ultrasonic | Beeps faster as you get closer |
| Hotel room light | PIR motion sensor | Turns off when you leave |
| Apple Watch | Heart rate + accelerometer | Tracks health, sends alerts |
| Smart doorbell | Camera + PIR | Notifies you when someone arrives |

---

## 🔄 The IoT Pipeline

**Every IoT system ever built follows this exact flow:**

```
┌──────────┐   ┌──────────┐   ┌───────────┐   ┌─────────┐   ┌───────────┐   ┌──────────┐
│  SENSOR  │──▶│  ESP32   │──▶│  INTERNET │──▶│  CLOUD  │──▶│ DASHBOARD │──▶│  ACTION  │
│  (Eyes)  │   │ (Brain)  │   │ (Highway) │   │(Memory) │   │  (Voice)  │   │ (Result) │
└──────────┘   └──────────┘   └───────────┘   └─────────┘   └───────────┘   └──────────┘
```

**Memory trick:**
- Sensors = **Eyes** — see the world
- ESP32 = **Brain** — decides what to do
- Cloud = **Memory** — stores everything
- Dashboard = **Voice** — shows insights

---

## 💻 Getting Started with Wokwi

[Wokwi](https://wokwi.com) is a **free browser-based ESP32 simulator**. No hardware, no installation, no cost.

### How to open a new project

1. Go to **[wokwi.com](https://wokwi.com)**
2. Click **Start from scratch**
3. Select **ESP32**
4. You'll see:
   - **Left panel** → code editor
   - **Right panel** → visual simulation
5. Press **▶ Play** to run your code
6. Open **Serial Monitor** at the bottom to see output

### Tips for Wokwi

- 🔁 **Restart** — click the ■ Stop button, then ▶ Play again after changing code
- 📌 **Save your project** — sign in to save and share your simulations
- 🔗 **Share** — click Share to get a link others can open
- ⚠️ **WiFi in Wokwi** — use the built-in simulated WiFi (no real credentials needed in simulation)

---

---

## ⚡ Chapter 1 — ESP32 Basics

### About the ESP32

| Feature | Value |
|---|---|
| Processor | Dual-core Xtensa LX6 @ 240 MHz |
| RAM | 520 KB |
| Connectivity | WiFi 802.11 b/g/n + Bluetooth 4.2 + BLE |
| GPIO Pins | 34+ general purpose pins |
| ADC | 12-bit (reads 0–4095) |
| Operating Voltage | 3.3V |
| Cost | ~₹250 |

### Why ESP32 for IoT?

- Built-in **WiFi + Bluetooth** — no extra modules needed
- **Dual core** — can run communication and logic simultaneously
- **Large community** — thousands of free libraries and tutorials
- **Cheap** — professional-grade for hobby price

---

---

## 🔌 Chapter 2 — GPIO & PWM

### GPIO — General Purpose Input Output

Every pin on the ESP32 can be set as:

```
INPUT  → reads from the world  (buttons, sensors)
OUTPUT → controls something    (LEDs, buzzers, relays)
```

```
HIGH = 3.3V   (logic 1)
LOW  = 0V     (logic 0)
```

### Key Functions

```cpp
pinMode(pin, OUTPUT);      // configure pin as output
pinMode(pin, INPUT);       // configure pin as input
pinMode(pin, INPUT_PULLUP);// input with internal pull-up resistor

digitalWrite(pin, HIGH);   // set output HIGH (3.3V)
digitalWrite(pin, LOW);    // set output LOW (0V)
int val = digitalRead(pin);// read input: returns HIGH or LOW
```

---

### Code 1 — LED Blink

```cpp
void setup() {
  pinMode(2, OUTPUT);  // pin 2 = output
}

void loop() {
  digitalWrite(2, HIGH);  // LED on
  delay(500);              // wait 500ms
  digitalWrite(2, LOW);   // LED off
  delay(500);
}
```

**Try these challenges:**
- Change `500` to `100` — what happens?
- Change `500` to `2000` — what happens?
- Make the LED blink 3 times fast, then pause for 2 seconds

---

### Code 2 — Button Controls LED

```cpp
#define LED_PIN    2
#define BUTTON_PIN 15

void setup() {
  pinMode(LED_PIN,    OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP); // LOW = pressed
}

void loop() {
  int btn = digitalRead(BUTTON_PIN);

  if (btn == LOW) {           // button pressed
    digitalWrite(LED_PIN, HIGH);
  } else {                    // button released
    digitalWrite(LED_PIN, LOW);
  }
}
```

**Try this challenge:**
- Make the LED toggle (press once = on, press again = off) using a variable

---

### PWM — Simulating Analog Output

ESP32 is digital — it can only output 0V or 3.3V. **PWM** (Pulse Width Modulation) switches the pin on/off so fast (5000×/sec) that the LED sees an average voltage.

```
Duty 0%   ████░░░░░░░░░░░░  =  0% brightness
Duty 50%  ████████░░░░░░░░  =  50% brightness  
Duty 100% ████████████████  =  100% brightness
```

### Key PWM Functions

```cpp
ledcSetup(channel, frequency, resolution);
// channel    = 0–15  (virtual PWM lane)
// frequency  = Hz    (5000 is good for LEDs)
// resolution = bits  (8-bit = values 0–255)

ledcAttachPin(pin, channel);   // connect physical pin to channel
ledcWrite(channel, dutyCycle); // set brightness 0–255
```

---

### Code 3 — PWM Auto-Fade

```cpp
void setup() {
  ledcSetup(0, 5000, 8);   // channel 0, 5kHz, 8-bit
  ledcAttachPin(2, 0);      // pin 2 on channel 0
}

void loop() {
  // fade in (0 → 255)
  for (int i = 0; i <= 255; i++) {
    ledcWrite(0, i);
    delay(8);
  }
  // fade out (255 → 0)
  for (int i = 255; i >= 0; i--) {
    ledcWrite(0, i);
    delay(8);
  }
}
```

**Try this challenge:**
- Change `delay(8)` to `delay(2)` — faster fade
- Change `delay(8)` to `delay(20)` — slower fade
- Make it pause at full brightness for 1 second before fading out

---

---

## 🔬 Chapter 3 — Sensors

> **Remember:** Sensor = Input · ESP32 = Brain · Output = Decision

---

### Sensor 1 — DHT11 (Temperature & Humidity)

**Used in:** Smart ACs, weather stations, greenhouses, server room monitoring

**Output:** Temperature in °C, Humidity in %

#### Wiring

```
DHT11 VCC  → 3.3V
DHT11 DATA → GPIO 4
DHT11 GND  → GND
```

#### Code — Basic Reading

```cpp
#include <DHT.h>

#define DHTPIN  4
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
}

void loop() {
  float temp = dht.readTemperature(); // °C
  float hum  = dht.readHumidity();   // %

  if (isnan(temp) || isnan(hum)) {
    Serial.println("Read failed!");
    delay(2000);
    return;
  }

  Serial.print("Temp: "); Serial.print(temp); Serial.println(" °C");
  Serial.print("Hum:  "); Serial.print(hum);  Serial.println(" %");
  delay(2000);
}
```

#### Code — With Alert Logic

```cpp
#include <DHT.h>

#define DHTPIN  4
#define DHTTYPE DHT11
#define LED_PIN 2

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  dht.begin();
}

void loop() {
  float temp = dht.readTemperature();
  float hum  = dht.readHumidity();

  if (isnan(temp) || isnan(hum)) { delay(2000); return; }

  if (temp > 30) {
    Serial.println("🔥 HEAT ALERT!");
    digitalWrite(LED_PIN, HIGH);
  } else if (hum > 70) {
    Serial.println("💧 HUMIDITY ALERT!");
    digitalWrite(LED_PIN, HIGH);
  } else {
    Serial.println("✅ All good");
    digitalWrite(LED_PIN, LOW);
  }

  Serial.print("Temp: "); Serial.print(temp); Serial.println("°C");
  Serial.print("Hum:  "); Serial.print(hum);  Serial.println("%");
  delay(2000);
}
```

**Challenges:**
- Add a third condition: if `temp < 15` → print `"Too cold!"`
- Change the threshold to match your actual room temperature

---

### Sensor 2 — LDR (Light Sensor)

**Used in:** Automatic street lights, screen brightness, camera exposure

**Output:** Analog value 0 (dark) to 4095 (bright)

#### Wiring

```
3.3V → LDR → Analog Pin A0 → 10kΩ resistor → GND
```

#### Code — Basic LDR

```cpp
#define LDR_PIN A0
#define LED_PIN 2

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  int light = analogRead(LDR_PIN); // 0 = dark, 4095 = bright

  Serial.print("Light: "); Serial.println(light);

  if (light < 500) {
    digitalWrite(LED_PIN, HIGH); // dark → LED ON
    Serial.println("🌙 Dark");
  } else {
    digitalWrite(LED_PIN, LOW);  // bright → LED OFF
    Serial.println("☀️ Bright");
  }

  delay(200);
}
```

**Challenges:**
- Print the actual LDR value first to find YOUR room's threshold
- Map light level to LED brightness: dark room = bright LED, bright room = dim LED

---

### Sensor 3 — HC-SR04 (Ultrasonic Distance)

**Used in:** Car parking assist, robot obstacle avoidance, liquid level sensors

**Output:** Distance in centimetres

#### How it works

```
1. Send 10μs pulse on TRIG pin
2. Sound travels out at 343 m/s
3. Echo bounces back from object
4. Measure echo time on ECHO pin
5. Distance (cm) = time (μs) ÷ 58
```

#### Wiring

```
HC-SR04 VCC  → 5V
HC-SR04 TRIG → GPIO 5
HC-SR04 ECHO → GPIO 18
HC-SR04 GND  → GND
```

#### Code — Distance Meter

```cpp
#define TRIG_PIN 5
#define ECHO_PIN 18

void setup() {
  Serial.begin(9600);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

long measureDistance() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  long duration = pulseIn(ECHO_PIN, HIGH);
  return duration / 58; // convert to cm
}

void loop() {
  long dist = measureDistance();
  Serial.print("Distance: ");
  Serial.print(dist);
  Serial.println(" cm");

  if (dist < 10) {
    Serial.println("⚠️ Object too close!");
  }

  delay(500);
}
```

**Why divide by 58?**  
Sound travels at 0.0343 cm/μs. The signal makes a round trip (out + back), so:  
`Distance = (duration × 0.0343) ÷ 2 = duration ÷ 58.3 ≈ duration ÷ 58`

**Challenges:**
- Add an LED that turns on when distance < 10 cm
- Make an LED blink faster as the object gets closer (map distance to delay)

---

### Sensor 4 — PIR (Motion Sensor)

**Used in:** Security lights, burglar alarms, automatic doors, occupancy detection

**Output:** Digital HIGH (motion) or LOW (no motion)

**Important:** PIR needs ~2 seconds to calibrate after power-on. Add `delay(2000)` in `setup()`.

#### Wiring

```
PIR VCC → 5V
PIR OUT → GPIO 13
PIR GND → GND
```

#### Code — Motion Detector

```cpp
#define PIR_PIN 13
#define LED_PIN 2

void setup() {
  Serial.begin(9600);
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  Serial.println("Calibrating PIR...");
  delay(2000); // wait for PIR to stabilise
  Serial.println("Ready!");
}

void loop() {
  int motion = digitalRead(PIR_PIN);

  if (motion == HIGH) {
    Serial.println("⚠️ Motion Detected!");
    digitalWrite(LED_PIN, HIGH);
    delay(3000);           // keep LED on 3 seconds
    digitalWrite(LED_PIN, LOW);
  }
}
```

#### Code — Motion Counter

```cpp
#define PIR_PIN 13
#define LED_PIN 2

int count = 0;
bool wasTriggered = false;

void setup() {
  Serial.begin(9600);
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  delay(2000);
}

void loop() {
  bool motion = digitalRead(PIR_PIN);

  if (motion && !wasTriggered) {
    count++;
    wasTriggered = true;
    Serial.print("Motion #"); Serial.println(count);
    digitalWrite(LED_PIN, HIGH);

    if (count > 5) {
      Serial.println("🚨 High activity area!");
    }
  }

  if (!motion) {
    wasTriggered = false;
    digitalWrite(LED_PIN, LOW);
  }

  delay(100);
}
```

**Challenges:**
- Add a timestamp (use `millis()/1000` for seconds since start)
- Reset the counter every 60 seconds and print a summary

---

### Sensor 5 — Potentiometer

**Used in:** Volume knobs, brightness dimmers, motor speed control, servo positioning

**Output:** Analog value 0–4095 based on knob position

#### Wiring

```
Left pin   → 3.3V
Middle pin → GPIO 35 (analog input)
Right pin  → GND
```

#### Code — Pot to LED Brightness

```cpp
#define POT_PIN 35
#define LED_PIN 2

void setup() {
  Serial.begin(9600);
  ledcSetup(0, 5000, 8);
  ledcAttachPin(LED_PIN, 0);
}

void loop() {
  int potVal   = analogRead(POT_PIN);             // 0–4095
  int duty     = map(potVal, 0, 4095, 0, 255);    // 0–255

  ledcWrite(0, duty);

  Serial.print("Pot: "); Serial.print(potVal);
  Serial.print(" | Brightness: "); Serial.print(duty);
  Serial.print(" (");
  Serial.print(map(duty, 0, 255, 0, 100));
  Serial.println("%)");

  delay(50);
}
```

**Challenges:**
- Map the pot to control the blink speed of an LED instead of brightness
- Use the pot to choose between 5 modes (divide 0–4095 into 5 zones)

---

---

## ☁️ Chapter 4 — WiFi & ThingSpeak Cloud

### Connecting to WiFi

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

  Serial.println("\nConnected!");
  Serial.println("IP: " + WiFi.localIP().toString());
}
```

---

### Setting Up ThingSpeak

**ThingSpeak** is a free IoT cloud platform that stores your sensor data and draws live graphs.

#### Step-by-Step

**1.** Go to **[thingspeak.com](https://thingspeak.com)** → Sign Up Free

**2.** Create a Channel:
- Click **Channels → New Channel**
- Name: `ESP32 Weather Station`
- Field 1: `Temperature`
- Field 2: `Humidity`
- Click **Save Channel**

**3.** Get your API Key:
- Click the **API Keys** tab
- Copy your **Write API Key**
- Looks like: `A1B2C3D4E5F6G7H8`
- ⚠️ Never share this publicly

**4.** Understand the URL:

```
http://api.thingspeak.com/update?api_key=YOUR_KEY&field1=27.5&field2=65
│                          │     │              │        │          │
│                          │     │              │        │          └── Field 2 value
│                          │     │              │        └── Field 1 value
│                          │     │              └── Your secret key
│                          │     └── Parameters start
│                          └── Endpoint
└── ThingSpeak server
```

**5.** ThingSpeak response codes:

| Code | Meaning |
|---|---|
| `1`, `2`, `3`... | ✅ Success |
| `-1` | ❌ No internet |
| `400` | ❌ Wrong API key |
| `0` | ❌ Too many requests (wait 15 sec) |

---

### Full Code — DHT11 to ThingSpeak

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <DHT.h>

// WiFi credentials
const char* ssid = "YOUR_WIFI_NAME";
const char* pass = "YOUR_WIFI_PASSWORD";

// ThingSpeak — paste your Write API Key here
String apiKey = "YOUR_WRITE_API_KEY";

// DHT11 setup
#define DHTPIN  4
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(115200);
  dht.begin();

  // Connect to WiFi
  WiFi.begin(ssid, pass);
  Serial.print("Connecting to WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\n✅ Connected!");
}

void uploadToThingSpeak(float temp, float hum) {
  if (WiFi.status() != WL_CONNECTED) {
    Serial.println("No WiFi!");
    return;
  }

  // Build the URL
  String url = "http://api.thingspeak.com/update"
               "?api_key=" + apiKey +
               "&field1="  + String(temp) +
               "&field2="  + String(hum);

  HTTPClient http;
  http.begin(url);
  int code = http.GET();
  http.end();

  if (code > 0) {
    Serial.println("✅ Uploaded! Entry #" + String(code));
  } else {
    Serial.println("❌ Failed. Code: " + String(code));
  }
}

void loop() {
  float temp = dht.readTemperature();
  float hum  = dht.readHumidity();

  if (isnan(temp) || isnan(hum)) {
    Serial.println("Sensor error. Retrying...");
    delay(2000);
    return;
  }

  Serial.print("Temp: "); Serial.print(temp); Serial.println("°C");
  Serial.print("Hum:  "); Serial.print(hum);  Serial.println("%");

  uploadToThingSpeak(temp, hum);

  // ThingSpeak free tier: 1 update per 15 seconds minimum
  delay(20000);
}
```

---

---

## 🚀 Chapter 5 — Build Your Project

### What every project needs

| Requirement | Example |
|---|---|
| ≥ 2 sensors | DHT11 + PIR |
| Logic condition | `if (temp > 30 && motion)` |
| Output | LED, buzzer, Serial message |
| Cloud | Real ThingSpeak upload or simulation |

### Cloud Simulation (if WiFi unavailable)

```cpp
// Add this instead of the real HTTP upload code
void simulateUpload(float temp, float hum) {
  Serial.println("--- Cloud Upload Simulation ---");
  Serial.println("URL: api.thingspeak.com/update");
  Serial.println("  field1 (temp) = " + String(temp));
  Serial.println("  field2 (hum)  = " + String(hum));
  Serial.println("Response: 200 OK | Entry saved ✅");
  Serial.println("-------------------------------");
}
```

### Demo Checklist

Before demoing your project, answer these 3 questions:

- [ ] **What does it do?** — one sentence explanation
- [ ] **How does it work?** — name the sensors and the logic
- [ ] **Where does the data go?** — cloud / dashboard / alert

---

### Project Templates

#### Smart Room Automation (LDR + PIR)

```
LOGIC:
if (dark AND motion present) → light ON
if (bright OR no motion)     → light OFF

SENSORS: LDR (A0) + PIR (GPIO 13)
OUTPUT:  LED (GPIO 2)
CLOUD:   field1=lightLevel, field2=motionDetected
```

#### Smart Weather Station (DHT11)

```
LOGIC:
if (temp > 30)  → HEAT ALERT
if (temp < 15)  → COLD ALERT  
if (hum  > 80)  → HUMIDITY ALERT

SENSORS: DHT11 (GPIO 4)
OUTPUT:  Serial + LED
CLOUD:   field1=temperature, field2=humidity
```

#### Smart Parking System (Ultrasonic)

```
LOGIC:
if (distance < 15cm) → spot OCCUPIED → red LED
if (distance >= 15cm)→ spot AVAILABLE → green LED

SENSORS: HC-SR04 (TRIG=5, ECHO=18)
OUTPUT:  Red LED + Green LED
CLOUD:   field1=distance, field2=occupied(0/1)
```

#### Smart Security System (PIR + DHT11)

```
LOGIC:
if (motion detected)   → ALERT + buzzer
if (temp > 40)         → OVERHEAT ALERT

SENSORS: PIR (GPIO 13) + DHT11 (GPIO 4)
OUTPUT:  LED + Buzzer
CLOUD:   field1=motion(0/1), field2=temp, field3=alertCount
```

---

---

## 📌 Quick Reference Card

### Pin Cheat Sheet

| What | Typical Pin | Type |
|---|---|---|
| Built-in LED | 2 | Digital Output |
| Button | 15 | Digital Input (INPUT_PULLUP) |
| DHT11 data | 4 | Digital Input |
| PIR output | 13 | Digital Input |
| Ultrasonic TRIG | 5 | Digital Output |
| Ultrasonic ECHO | 18 | Digital Input |
| LDR analog | A0 | Analog Input |
| Potentiometer | 35 | Analog Input |

### Common Functions

```cpp
// Digital I/O
pinMode(pin, OUTPUT / INPUT / INPUT_PULLUP);
digitalWrite(pin, HIGH / LOW);
int val = digitalRead(pin);        // returns HIGH or LOW

// Analog
int val = analogRead(pin);         // returns 0–4095
int mapped = map(val, 0, 4095, 0, 255); // rescale range

// PWM
ledcSetup(channel, 5000, 8);       // setup PWM
ledcAttachPin(pin, channel);        // attach pin
ledcWrite(channel, 0–255);          // set brightness

// Timing
delay(ms);                          // pause (blocks everything)
unsigned long t = millis();         // ms since boot (non-blocking)

// Serial
Serial.begin(9600);                 // start serial (in setup)
Serial.print("text");               // print without newline
Serial.println("text");             // print with newline

// WiFi
WiFi.begin(ssid, pass);            // connect to WiFi
WiFi.status() == WL_CONNECTED;     // check connection
WiFi.localIP().toString();          // get IP address

// DHT11
dht.readTemperature();              // °C, returns float
dht.readHumidity();                 // %, returns float
isnan(value);                       // true if read failed
```

### ThingSpeak Upload Template

```cpp
String url = "http://api.thingspeak.com/update"
             "?api_key=" + apiKey +
             "&field1="  + String(value1) +
             "&field2="  + String(value2);

HTTPClient http;
http.begin(url);
int code = http.GET();  // > 0 = success
http.end();
delay(20000);           // wait 20 seconds (free tier limit)
```

---

---

## 📚 What to Learn Next

### Immediate Next Steps

| Topic | Resource |
|---|---|
| More sensors (soil, rain, gas) | [randomnerdtutorials.com](https://randomnerdtutorials.com) |
| OLED display (SSD1306) | [lastminuteengineers.com](https://lastminuteengineers.com) |
| MQTT protocol (faster than HTTP) | [hivemq.com/mqtt-essentials](https://www.hivemq.com/mqtt-essentials/) |
| Blynk mobile dashboard | [blynk.io/getting-started](https://blynk.io/getting-started) |
| Firebase real-time database | [firebase.google.com](https://firebase.google.com) |

### Build These Next

| Project | Skills You'll Learn |
|---|---|
| Smart door lock (RFID) | SPI communication |
| Soil moisture monitor | Capacitive sensing, automated control |
| GPS tracker | UART, NEO-6M GPS |
| OLED weather display | I2C, graphics library |
| Bluetooth LED controller | ESP32 BLE, mobile app |
| Energy meter | CT sensor, power maths |

### Real Hardware Shopping List

| Component | Price | Where to Buy |
|---|---|---|
| ESP32 DevKit | ₹250 | Robocraze / Amazon India |
| DHT11 | ₹30 | Robocraze |
| HC-SR04 | ₹50 | Robocraze |
| PIR HC-SR501 | ₹40 | Robocraze |
| LDR | ₹10 | Local electronics store |
| Breadboard | ₹60 | Robocraze |
| Jumper wires | ₹50 | Robocraze |
| LEDs + resistors | ₹40 | Any electronics shop |
| **Total** | **~₹530** | Full workshop kit |

---

## 🆘 Stuck? Common Fixes

| Problem | Fix |
|---|---|
| LED not blinking | Check `pinMode(2, OUTPUT)` is in `setup()` |
| DHT reads NaN | Check DHTPIN matches your wiring. Add `delay(2000)` after `dht.begin()` |
| WiFi not connecting | Case-sensitive SSID/password. Try printing status in a loop |
| ThingSpeak returns -1 | No internet. Check WiFi is connected first |
| ThingSpeak returns 400 | Wrong API key. Copy again from ThingSpeak, check for spaces |
| ThingSpeak returns 0 | Too fast — add `delay(20000)` before next upload |
| PIR never triggers | Add `delay(2000)` at end of `setup()` for calibration |
| Serial shows nothing | Check `Serial.begin(9600)` is in `setup()`, match baud rate in monitor |

---

<div align="center">

**Happy building! 🚀**

*The world needs IoT engineers. You're one of them now.*

[⭐ Star this repo](https://github.com/your-username/iot-workshop) · [🐛 Report an issue](https://github.com/your-username/iot-workshop/issues) · [💬 Ask a question](https://github.com/your-username/iot-workshop/discussions)

</div>
