# 🪖 Shield-Sense — Smart Safety Helmet Dashboard

![Shield-Sense Banner](https://img.shields.io/badge/Shield--Sense-Safety%20Monitoring-F97316?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0xMiAyQzYuNDggMiAyIDYuNDggMiAxMnM0LjQ4IDEwIDEwIDEwIDEwLTQuNDggMTAtMTBTMTcuNTIgMiAxMiAyeiIvPjwvc3ZnPg==)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![ESP32](https://img.shields.io/badge/ESP32-IoT-0EA5E9?style=for-the-badge)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Deployed-22C55E?style=for-the-badge&logo=github)

> Real-time IoT safety monitoring dashboard for construction and mining environments. Live sensor data from an ESP32-powered smart helmet, streamed to Firebase and displayed on a sleek web dashboard.

---

## 🔴 Live Demo

> **[https://YOUR-USERNAME.github.io/shield-sense-dashboard/](https://Atharva-Patil9.github.io/shield-sense-dashboard/)**

---

## 📸 Features

| Feature | Description |
|---|---|
| 🔐 **Login** | Firebase Auth — email/password protected |
| 💨 **Gas Monitoring** | Live MQ-2 readings with Safe / Warning / Danger levels |
| 🌡 **Temperature** | Live DHT11 temp with alert above 45°C |
| 💧 **Humidity** | Live DHT11 humidity with alert above 85% |
| 🪖 **Fall Detection** | MPU6050 G-force spike and freefall detection |
| 🔔 **Toast Alerts** | Pop-up notification when fall is detected |
| 📋 **Alert History** | Full log of all past alerts from Firestore |
| ⏱ **Live Updates** | Data refreshes every 3 seconds from ESP32 |

---

## 🧰 Hardware

| Component | Purpose | GPIO Pin |
|---|---|---|
| ESP32 | Main controller + WiFi | — |
| MQ-2 Gas Sensor | Detects LPG, smoke, CO | GPIO34 (AO) |
| DHT11 | Temperature & Humidity | GPIO4 |
| MPU6050 | Accelerometer / Fall Detection | GPIO21 (SDA), GPIO22 (SCL) |
| MT3608 Boost Converter | Steps 3.7V → 5V for MQ-2 | — |
| 18650 + TP4056 | Battery + charging circuit | — |

---

## ⚡ Alert Thresholds

| Sensor | Warning | Danger |
|---|---|---|
| Gas (MQ-2) | > 400 ppm | > 600 ppm |
| Temperature | > 45 °C | — |
| Humidity | > 85 % | — |
| Fall (G-force spike) | — | > 2.5 G |
| Fall (Freefall) | — | < 0.3 G |

---

## 🏗 Project Architecture

```
ESP32 (reads sensors every 3s)
       │
       ├──► Firebase RTDB ──────────► Dashboard Live Monitoring
       │    /helmet_data              (gas, temp, humidity, fall)
       │
       └──► Firestore ──────────────► Dashboard Alert History
            /alerts collection        (type, message, timestamp)
```

---

## 📁 Repository Structure

```
shield-sense-dashboard/
│
├── index.html          # Full dashboard (login + live + alerts)
└── README.md           # This file
```

---

## 🚀 Setup & Deployment

### 1. Clone the Repository
```bash
git clone https://github.com/YOUR-USERNAME/shield-sense-dashboard.git
cd shield-sense-dashboard
```

### 2. Firebase Setup
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create project **shield-sense**
3. Enable **Realtime Database** (asia-southeast1 region)
4. Enable **Firestore Database**
5. Enable **Authentication → Email/Password**
6. Create a user under **Authentication → Users**

### 3. Firebase Rules

**Realtime Database Rules:**
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

**Firestore Rules:**
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

### 4. Deploy to GitHub Pages
1. Push code to GitHub
2. Go to **Settings → Pages**
3. Source → **Deploy from branch → main → / (root)**
4. Add your GitHub Pages URL to **Firebase Console → Authentication → Authorized Domains**

---

## 🔧 ESP32 Arduino Libraries

Install these in Arduino IDE via **Library Manager:**

| Library | Author |
|---|---|
| `Firebase ESP Client` | Mobizt |
| `DHT sensor library` | Adafruit |
| `Adafruit Unified Sensor` | Adafruit |
| `MPU6050` | Electronic Cats |
| `ArduinoJson` | Benoit Blanchon |

---

## 📡 Firebase RTDB Data Structure

```json
{
  "helmet_data": {
    "gas_level":     450,
    "temperature":   32.5,
    "humidity":      68.0,
    "fall_detected": false,
    "last_updated":  1234567890
  }
}
```

## 📋 Firestore Alert Log Structure

```
/alerts (collection)
  /alert_1_timestamp
    fields:
      type:      "gas_leak_danger"
      message:   "Gas level critically high: 720"
      timestamp: 1234567890
```

---

## ⚠️ Current Limitations

- **WiFi Dependency** — No offline mode; data stops if signal drops
- **Battery Life** — Single 18650 has limited runtime with WiFi active
- **DHT11 Accuracy** — ±2°C and ±5% humidity tolerance
- **MQ-2 Warmup** — Needs 2-3 minutes after power-on for stable readings
- **Fall Detection** — May trigger false positives on sudden head movements
- **Firebase Rules** — Currently open (test mode); lock down before production

---

## 🗺 Roadmap

- [x] ESP32 hardware wiring
- [x] Sensor reading code (MQ-2, DHT11, MPU6050)
- [x] Firebase RTDB push
- [x] Web dashboard — Login, Live Monitoring, Alert History
- [x] GitHub Pages deployment
- [ ] Fix Firestore alert logging
- [ ] Fall detection threshold tuning
- [ ] Battery deep sleep optimization
- [ ] Secure Firebase rules
- [ ] Multiple helmet support

---

## 👨‍💻 Tech Stack

**Hardware:** ESP32 · MQ-2 · DHT11 · MPU6050 · 18650 Battery

**Firmware:** Arduino C++ · Firebase ESP Client · Adafruit DHT · Electronic Cats MPU6050

**Backend:** Firebase Realtime Database · Cloud Firestore · Firebase Auth

**Frontend:** HTML · CSS · JavaScript · Firebase JS SDK v10

**Hosting:** GitHub Pages

---

## 📄 License

MIT License — feel free to use, modify, and distribute.
