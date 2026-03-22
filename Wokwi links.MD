# 🔗 Wokwi Simulation Links

> **Open any project below in one click — no setup, no installation, no hardware needed.**  
> All simulations run entirely in your browser at [wokwi.com](https://wokwi.com).

---

## ⚡ How to Use These Links

1. Click any link below
2. The simulation opens in your browser
3. Press **▶ Play** to run it
4. Open **Serial Monitor** at the bottom to see output
5. **Edit the code** freely — your changes don't affect the original
6. **Save your version** by signing in and clicking Fork

---

## 📋 Session Links

### 🔵 Session 2 — GPIO & PWM

| Project | Description | Link |
|---|---|---|
| LED Blink | Basic ON/OFF with `delay()` | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| Button Controls LED | `INPUT_PULLUP` + `digitalRead()` | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| PWM Auto-Fade | LED breathes in/out using `ledcWrite()` | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |

### 🟠 Session 3 — Sensor Playground

| Sensor | Description | Link |
|---|---|---|
| DHT11 — Basic Reading | Temperature + humidity to Serial Monitor | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| DHT11 — With Alert | Heat + humidity threshold conditions | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| LDR — Light Sensor | Auto LED based on ambient light | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| HC-SR04 — Ultrasonic | Distance measurement in cm | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| PIR — Motion Sensor | Detect movement, trigger LED | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| Potentiometer | Analog knob → LED brightness via PWM | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |

### 🟢 Session 4 — ThingSpeak Cloud

| Project | Description | Link |
|---|---|---|
| WiFi Connect | Connect ESP32 to WiFi, print IP | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| DHT11 → ThingSpeak | Full upload with validation | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |

---

## 🚀 Hackathon Project Links

| Project | Sensors | Link |
|---|---|---|
| Smart Room Automation | LDR + PIR + LED | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| Smart Parking System | HC-SR04 + LEDs | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| Smart Weather Station | DHT11 + ThingSpeak | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| Smart Security System | PIR + DHT11 + Buzzer | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |
| Assistive Smart Helmet ⭐ | DHT11 + PIR + Ultrasonic | [▶ Open in Wokwi](https://wokwi.com/projects/new/esp32) |

---

## 📝 How to Add Your Own Link

After building your project in Wokwi:

1. Click **Share** (top right)
2. Copy the project URL
3. Open a Pull Request to add it to this file

### Link format:
```markdown
| Your Project Name | Description | [▶ Open in Wokwi](https://wokwi.com/projects/YOUR_PROJECT_ID) |
```

---

## 🛠️ Building Your Own Wokwi Projects

### Starting from scratch

1. Go to [wokwi.com](https://wokwi.com)
2. Click **Start from scratch → ESP32**
3. In the editor, click **+** to add components
4. Search for: `DHT11`, `HC-SR04`, `PIR`, `LDR`, `Potentiometer`, `LED`, `Push Button`
5. Wire components by clicking pins and dragging connections
6. Paste code from the [README](./README.md) or [STUDENT_GUIDE](./STUDENT_GUIDE.md)
7. Press **▶ Play**

### Wokwi keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + Enter` | Start / Stop simulation |
| `Ctrl + S` | Save project |
| `Ctrl + Z` | Undo |
| `F1` | Command palette |
| `Delete` | Remove selected component |

### Wokwi tips

- **Serial Monitor** — click the monitor icon at the bottom during simulation
- **Baud Rate** — must match `Serial.begin()` in your code (usually `9600` or `115200`)
- **WiFi** — Wokwi simulates WiFi automatically, no real credentials needed
- **DHT11 values** — click the sensor during simulation to change temperature/humidity interactively
- **Pause** — hover over the simulation area to see the current state of each component

---

## 🔧 Component Wiring Quick Reference

Use this when building from scratch in Wokwi.

### LED

```
ESP32 GPIO 2 → 220Ω resistor → LED (+) → LED (-) → GND
```

### Button (with INPUT_PULLUP)

```
ESP32 GPIO 15 → Button → GND
(INPUT_PULLUP: pin reads LOW when pressed)
```

### DHT11

```
VCC  → 3.3V
DATA → GPIO 4
GND  → GND
```

### HC-SR04 Ultrasonic

```
VCC  → 5V
TRIG → GPIO 5
ECHO → GPIO 18
GND  → GND
```

### PIR Motion Sensor

```
VCC → 5V
OUT → GPIO 13
GND → GND
```

### LDR (voltage divider)

```
3.3V → LDR → A0 pin → 10kΩ → GND
```

### Potentiometer

```
Left  → 3.3V
Mid   → GPIO 35
Right → GND
```

---

## 📌 Note on Links

> The links in this file currently point to `wokwi.com/projects/new/esp32` (blank new project).  
> **To get real pre-built simulation links:** build each project in Wokwi, save it, copy the share link, and replace the placeholder links above.  
>
> If you've built these projects and want to contribute your links, open a Pull Request! 🙌

---

<div align="center">

**Built for the IoT Workshop — ESP32 + Wokwi + ThingSpeak**

[📘 Student Guide](./STUDENT_GUIDE.md) · [📖 README](./README.md) · [🎤 Instructor Script](./scripts/instructor_script.md)

</div>
