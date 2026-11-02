# Smart Cane for Visually Impaired with AI Obstacle Detection

A low-cost smart cane prototype. An ESP32 on the cane measures distance and wet ground and warns the user with a buzzer. A phone web page recognises everyday objects through the camera and shows a confidence score for each one. A caretaker web page shows the live information from anywhere.

Engineering Clinics project, B.Tech CSE (AI & ML), CGC University, Mohali.

**Live pages**
- Phone page (camera and AI): https://smart-cane-site.netlify.app/
- Caretaker page: https://smart-cane-site.netlify.app/caretaker.html

## What it does
- **On the cane (ESP32):** an ultrasonic sensor measures how far an object is, and a buzzer beeps faster as the object gets closer. A water sensor gives a separate warning for wet ground.
- **On the phone (web page):** the camera feed is analysed in the browser by a pre-trained object detector (COCO-SSD on TensorFlow.js). Each object is shown with its name, confidence score, position (left, ahead, right) and how close it looks.
- **For the caretaker (web page):** shows whether the cane is online, a hazard banner, the distance and wet-ground reading, what the phone camera sees, the user's location on a map, and a log of recent alerts.

## How it fits together
Cane (ESP32) and phone each send their data to a Firebase Realtime Database. The caretaker page reads it from there every 2 seconds.

## Files
| File | What it is |
|---|---|
| `index.html` | Phone page: live object detection, sharing settings |
| `caretaker.html` | Caretaker dashboard |

The ESP32 firmware (Arduino C++) is kept separately.

## Set it up yourself
1. Create a free Firebase project and a **Realtime Database**.
2. In the **Rules** tab, paste the rules below and publish.
3. Choose a long, random **pairing code** (for example `cane-` plus 8 random letters and numbers). The cane, the phone page and the caretaker page must all use the same code.
4. Host `index.html` and `caretaker.html` on any static host with HTTPS (GitHub Pages or Netlify). The camera only works on HTTPS pages.
5. On the phone page, enter the database address and the pairing code, then press **Save and test**.

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "cane":   { "$code": { ".read": true, ".write": true } },
    "events": { "$code": { ".read": true, ".write": true } }
  }
}
```

## Privacy and limits
- Camera pictures never leave the phone. Only object names, confidence scores and, if the user switches it on, the location are shared.
- Anyone who knows the database address **and** the pairing code can read and write that data. Keep the code private. This prototype has no user sign-in.
- The detector knows 80 everyday object types. It cannot see stairs, potholes or glass doors.
- This is a prototype that supports a white cane. It does not replace one.

## Built with
ESP32, HC-SR04 ultrasonic sensor, water sensor, buzzer, TensorFlow.js, COCO-SSD, HTML, CSS, JavaScript, Firebase Realtime Database, OpenStreetMap, GitHub, Netlify.
