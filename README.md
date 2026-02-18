<div align="center">

# Autónoma GPS

<p align="center">
  <img src="assets/images/home/logo_gps.png" width="220">
</p>

![Flutter](https://img.shields.io/badge/Flutter-3.19+-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-3.9-0175C2?style=for-the-badge&logo=dart&logoColor=white)
![Bluetooth](https://img.shields.io/badge/BLE-iBeacon-0082FC?style=for-the-badge&logo=bluetooth&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Android%20%7C%20iOS-3DDC84?style=for-the-badge&logo=android&logoColor=white)

### Final Degree Project - Computer Engineering 2024/2025 at Universidade Autónoma de Lisboa

*An **indoor GPS navigation system** for visually impaired students at UAL, providing real-time audio guidance.*

</div>

> ## Full Documentation
> This README provides a structured overview of the project. For a comprehensive and in-depth explanation of every technical decision, system architecture, beacon mapping, algorithm implementation, challenges faced, and future work **read the full report**:
>
> **[Access the Full Project Report](android/report/Report.pdf)** &nbsp;|&nbsp; **[Watch the Project Video](https://www.youtube.com/watch?v=wC5PRanA4iI)**
>

## About the Project

Independent mobility in indoor environments remains a major challenge for visually impaired people, especially in complex spaces like a university campus. **Autónoma GPS** was developed in direct response to this need, in collaboration with the **Office for Inclusion and University Resilience (GIRU)** at UAL and with the direct input of a blind student from the institution itself.

The app uses **Bluetooth Low Energy (BLE) Beacons** strategically placed across all floors of the building (Floor -1 to Floor 4) to determine the user's position in real time. Based on that location, the system calculates the most efficient route using **Dijkstra's Algorithm** and delivers clear, adapted audio instructions — enabling fully autonomous navigation indoors.

### Highlights
- **38 beacons** installed across 6 floors of the UAL campus
- Fully **hands-free navigation** via voice commands
- **Android & iOS** support from a single codebase
- Interface designed from **real feedback** from a blind user
- Audio instructions available in **Portuguese, English and French**

## Features

| Feature | Description |
|---------|-------------|
| **Navigation Mode** | Select a destination and receive step-by-step audio instructions |
| **Guided Tour Mode** | Explore campus points of interest with automatic narration |
| **Voice Commands** | Full app control without touching the screen |
| **Text-to-Speech (TTS)** | Real-time audio instructions with adjustable speed and volume |
| **Favourites** | Save frequent destinations for quick access |
| **Sound Settings** | Customise voice, language, speed and volume |
| **Accessibility Settings** | Adjust contrast, text size and vibration feedback |
| **Map Settings** | Configure the UAL indoor map display preferences |
| **Light / Dark Mode** | Visual themes for greater comfort |
| **Multilingual** | UI available in Portuguese, English and French via JSON files |
| **Privacy Policy** | Informed consent popup on first launch |

## System Architecture

```mermaid
flowchart TD
    A["App Launch"] --> B["Splash Screen"]
    B --> C["Home Page"]
    C --> D["Navigation Mode"]
    C --> E["Guided Tour Mode"]
    C --> F["Settings"]
    D --> D1["Destination Selection"]
    D1 --> D2["BLE Beacon Detection"]
    D2 --> D3["Dijkstra Algorithm"]
    D3 --> D4["TTS Audio Instructions"]
    D4 --> D5["Position Update"]
    D5 --> D2
    E --> E1["Tour Selection"]
    E1 --> E2["Guided Tour Active"]
    F --> F1["Sound & TTS"]
    F --> F2["Accessibility"]
    F --> F3["Map"]
    F --> F4["Personalisation"]
    F --> F5["Language"]

    classDef page fill:#3b3b3b,color:#fff,stroke:#888,stroke-width:1.5px
    classDef feature fill:#1e3a5f,color:#fff,stroke:#4a90d9,stroke-width:1.5px
    class A,B,C page
    class D,E,F,D1,D2,D3,D4,D5,E1,E2,F1,F2,F3,F4,F5 feature
```

## Pages & Functionality

### 1. Splash Screen

The initial screen displaying the **Autónoma GPS** visual identity, initialising the beacon connection and loading the user's saved preferences. On first launch, it also presents the Privacy Policy popup for informed consent.

### 2. Home Page

The central hub of the app, providing direct access to the two main modes — **Navigation** and **Guided Tour** — as well as the app settings.

### 3. Navigation Mode

The functional core of the system. The user selects a destination (by tap or voice command) and the app calculates the most efficient route based on the current position estimated by the nearest BLE beacons.

**How it works:**

1. The user selects a destination or accesses a saved favourite
2. The system detects the nearest BLE beacons and estimates position via RSSI
3. Dijkstra's Algorithm calculates the optimal route through the beacon graph
4. Audio instructions are delivered in real time via TTS
5. Position is continuously updated as the user moves
6. If the user deviates from the route, the system automatically recalculates

> 💡 Voice command mode enables a fully hands-free experience — users can select destinations, access favourites, and control navigation using only their voice.

### 4. Guided Tour Mode

Allows the user to follow a **pre-defined route** through the campus points of interest, with automatic narration at each location. Ideal for new students or visitors who want to explore the facilities independently.

> ⚠️ In the current version, the tour always starts from the main entrance. The ability to start from any point is planned for future development.

### 5. Settings

Full control over the app experience, organised into five sections:

| Section | What it configures |
|---------|--------------------|
| **Sound** | TTS voice, speed, volume, language and audio files |
| **Accessibility** | Contrast, text size, vibration and feedback type |
| **Map** | UAL indoor map display preferences |
| **Personalisation** | Light/dark theme and visual settings |
| **Language** | Portuguese / English / French |

---

## Hardware — BLE Beacons

The system uses **38 beacons** distributed across all 6 floors of the UAL building (Floor -1 to Floor 4), using Apple's **iBeacon** protocol — compatible with both Android and iOS.

| Model | Manufacturer | Role | Protocol | Battery |
|-------|-------------|------|----------|---------|
| **nRF51822** | DUOWEISI | Initial testing | iBeacon | CR2032 |
| **Y1 (nRF51822-15044)** | Holyiot | Final installation | iBeacon | CR2477 |

The Holyiot Y1 beacons were selected for the final deployment due to their **superior robustness, battery life, signal stability and ease of configuration** compared to the initial test models.

**Each beacon is identified by:**
- **UUID** — Unique identifier for the project's beacon set
- **Major** — Identifies the floor/zone within the building
- **Minor** — Identifies the specific location within the floor
- **RSSI / Tx Power** — Used to estimate the distance between the user and the beacon

**Floor distribution:**

| Floor | Beacons Installed |
|-------|-------------------|
| Floor -1 | Covered |
| Floor 0 | Covered |
| Floor 1 | Covered |
| Floor 2 | Covered |
| Floor 3 | Covered |
| Floor 4 | Covered |

> For detailed beacon placement maps and individual positioning, refer to the **[Full Project Report](#)**.

---

## Dijkstra's Algorithm

**Dijkstra's Algorithm** is used to calculate the most efficient route between the user's current position and the selected destination. In this project:

- Each **beacon** corresponds to a **node** in the graph
- Each **connection between beacons** is an **edge**, weighted by step count, physical barriers, or accessibility of the path
- The algorithm iteratively selects the node with the lowest accumulated cost, progressively building the optimal path

**Execution steps within the app:**

1. **Graph construction** — physical UAL map converted into a beacon graph
2. **Origin node** — beacon closest to the user (highest RSSI)
3. **Route calculation** — Dijkstra finds the lowest-cost path to the destination
4. **Instruction delivery** — ordered beacon list converted into audio directions
5. **Real-time adaptation** — route deviations trigger automatic recalculation

---

## Flutter Libraries

| Library | Purpose |
|---------|---------|
| **flutter_blue** | BLE beacon detection and signal reading |
| **flutter_tts** | Text-to-Speech for audio instructions |
| **speech_to_text** | Voice command recognition |
| **easy_localization** | Multilingual UI system via JSON |
| **provider** | Global state management and preferences |
| **shared_preferences** | Persistence of favourites and settings |

---

## Setup & Installation

### Prerequisites

- Flutter SDK 3.19+
- Android Studio (with emulator or physical Android device)
- For iOS: Mac with Xcode installed
- Device with active Bluetooth BLE (for testing with real beacons)

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/Tomasalexpt30/autonoma-gps.git

# 2. Navigate to the project folder
cd autonoma-gps

# 3. Install dependencies
flutter pub get

# 4. Run the app
flutter run
```

> **Note:** To test navigation features with real beacons, a physical Android device in developer mode is required, connected via USB to the computer running Android Studio.

---

## Budget

All hardware costs were fully covered by the **Universidade Autónoma de Lisboa**, as part of the project grant application.

| Component | Brand | Qty | Unit Cost | Subtotal |
|-----------|-------|-----|-----------|----------|
| Test Beacons | DUOWEISI | 3 | €4.92 | €14.76 |
| Final Beacons | Holyiot Y1 | 35 | €5.58 | €195.30 |
| CR2032 Batteries | Duracell | 3 | €2.63 | €7.90 |
| CR2477 Batteries | Panasonic | 40 | €4.12 | €164.80 |
| Shipping Fees | — | — | — | €18.96 |
| **Total** | | | | **~€401.72** |

---

## Challenges & Solutions

| # | Challenge | Solution |
|---|-----------|----------|
| 1 | Flutter installation with SDK version conflicts | In-depth review of official documentation and group troubleshooting sessions |
| 2 | No prior experience with the Dart language | Intensive self-study using official materials, online courses and tutorials |
| 3 | DUOWEISI beacons had weak signal and short battery life | Replaced with Holyiot Y1, which showed superior real-world performance |
| 4 | VS Code limited for testing on physical devices | Migrated to Android Studio with USB device mirroring support |
| 5 | App unable to detect or communicate with beacons in early tests | Deep study of manufacturer technical documentation and hands-on testing sessions |
| 6 | Navigation instructions insufficient for multiple approach directions | Redesigned data structure using triplets: `origin – midpoint – destination` |
| 7 | Beacons detected at long range caused positioning interference | Implemented RSSI threshold filtering to ignore weak/distant signals |

---

## Future Work

The system is fully functional, but the team identified several improvements for future versions:

**Immediate Improvements**
- Refine navigation instruction detail on some routes
- Improve beacon signal capture in outdoor campus areas
- Allow the Guided Tour to start from any point, not just the main entrance
- Enable resuming the tour after interruption without restarting from the beginning

**Inertial Sensors (Dead Reckoning)**
Integration of the device's accelerometer, gyroscope and magnetometer to estimate the user's position in areas without beacon coverage, increasing navigation robustness and continuity.

**Database (Firebase)**
Firebase integration for centralised map management, remote route updates, and future scalability of the system to other buildings.

**Artificial Intelligence**
Incorporate AI for dynamic route adaptation based on usage patterns, obstacle detection, and progressive personalisation of the navigation experience.

---

## Documentation & Media

<div align="center">

| | Resource | Description |
|-|----------|-------------|
| 📄 | **[Full Project Report](#)** | Complete technical documentation — architecture, beacon mapping, algorithm details, results and more |
| 📰 | **[News Coverage](#)** | Media coverage of the Autónoma GPS project |
| 💻 | **[GitHub Repository](https://github.com/Tomasalexpt30/autonoma-gps)** | Source code and project files |
| 🏛️ | **[GIRU — UAL](https://autonoma.pt/info-giru/)** | Office for Inclusion and University Resilience |

</div>

> If you want to understand the full depth of this project — every technical decision, the complete beacon layout, the interview with the blind student, the budget justification, and the academic conclusions — **the report is the place to go.**

---

## Technologies

| Technology | Purpose |
|------------|---------|
| **Flutter 3.19+** | Cross-platform UI framework (Android & iOS) |
| **Dart 3.9** | Core application language |
| **Bluetooth Low Energy (BLE)** | Indoor positioning via beacons |
| **iBeacon Protocol** | Beacon communication standard |
| **Dijkstra's Algorithm** | Optimal route calculation through the beacon graph |
| **Text-to-Speech (TTS)** | Real-time audio instructions |
| **Voice Recognition** | Hands-free control via voice commands |
| **JSON** | Route storage, translations and voice commands |
| **Visual Studio Code** | Initial development environment |
| **Android Studio** | Main IDE for build and physical device testing |
| **GitHub** | Version control and team collaboration |

---

## Authors

Project developed as part of the **Bachelor's Degree in Computer Engineering** at **Universidade Autónoma de Lisboa — Luís de Camões**.

| Student | Number |
|---------|--------|
| Tomás Fernandes Alexandre | 30011117 |
| Pedro Rafael Borlinhas Falcão | 30011093 |
| Nicolae Iachimovschi | 30011284 |
| Guilherme Monteiro Brito | 30010959 |

> **Supervisor:** Professor Doutor Mário Marques da Silva | **July 2025**

---

<div align="center">
  <sub>Built with care to promote inclusion at Universidade Autónoma de Lisboa</sub>
</div>


