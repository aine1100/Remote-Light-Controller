
---

# 💡 IoT Light Scheduler

An IoT-based light scheduling system that lets users control ON/OFF times for a light using a sleek web interface. The schedule is sent to an Arduino via **WebSocket** and **MQTT**, which toggles a relay to control the light. The frontend features a modern **light-mode UI with green accents**.

---

## 🗂️ Project Structure

```
iot-light-scheduler/
├── backend/               # WebSocket server
│   └── server.py
├── frontend/              # Web UI
│   ├── index.html
│   ├── script.js
│   └── style.css
├── light/                 # Arduino code
│   └── light.ino
├── subscriber/            # MQTT subscriber
│   └── subscriber.py
├── requirements.txt       # Python dependencies
```

---

## ✅ Prerequisites

### 🔌 Hardware
- Arduino UNO (or compatible board)
- Relay module (active-low preferred)
- Light/load connected to the relay

### 💻 Software
- Python 3.7+
- Arduino IDE
- Mosquitto MQTT broker
- Modern browser (e.g., Chrome, Firefox)

### 📦 Dependencies
```bash
pip install -r requirements.txt
```

---

## ⚙️ Setup Instructions

### 1. 🛰️ Install Mosquitto MQTT Broker

<details>
<summary>Windows</summary>

- Download from [Mosquitto](https://mosquitto.org/download/).
- Run installer and start service:  
  `net start mosquitto`
- Add Mosquitto path (e.g., `C:\Program Files\mosquitto`) to system `PATH`.

</details>

<details>
<summary>Linux (Ubuntu)</summary>

```bash
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
```

</details>

<details>
<summary>macOS</summary>

```bash
brew install mosquitto
brew services start mosquitto
```

</details>

---

### 2. 🔌 Set Up Arduino

- Connect relay module:
  - **VCC → 5V**
  - **GND → GND**
  - **IN → Pin 7** (or change in `light.ino`)
- Wire the light to the relay’s **NO** and **COM** terminals.
- Open `light/light.ino` in Arduino IDE.
- Upload to your Arduino.
- Note your serial port (e.g., `COM3`, `/dev/ttyACM0`).

---

### 3. 🔁 Configure Subscriber

In `subscriber/subscriber.py`, update the serial port:
```python
SERIAL_PORT = 'COM3'  # Update for your system
```

---

### 4. 📥 Install Python Dependencies

```bash
pip install -r requirements.txt
```

---

### 5. 🖥️ Preview

| Web Interface Screenshots |
|---------------------------|
| ![Screenshot 1](Screenshot%202025-04-30%20052756.png) |
| ![Screenshot 2](Screenshot%202025-04-30%20052736.png) |
| ![Screenshot 3](Screenshot%202025-04-30%20052746.png) |

---

## 🚀 Usage

### 1. Start Mosquitto

```bash
# Windows
net start mosquitto

# Linux
sudo systemctl start mosquitto

# macOS
brew services start mosquitto
```

### 2. Run WebSocket Server

```bash
cd backend
python server.py
```

### 3. Start MQTT Subscriber

```bash
cd subscriber
python subscriber.py
```

### 4. Serve the Frontend

```bash
cd frontend
python -m http.server 8000
```

Open your browser and visit:  
👉 [http://localhost:8000](http://localhost:8000)

---

## ⏰ Set the Schedule

- Enter **ON** and **OFF** times in the UI (e.g., `08:00` and `20:00`)
- Click **"Set Schedule"**
- UI will confirm the status
- The Arduino toggles the relay based on time

---

## 🧠 How It Works

| Component       | Role |
|----------------|------|
| **Frontend**    | Light UI to set schedules and send them via WebSocket |
| **WebSocket Server** | Receives schedule and publishes to MQTT topic `light/schedule` |
| **MQTT Subscriber**  | Listens to `light/schedule`, sends `1` (ON) or `0` (OFF) to Arduino |
| **Arduino**     | Reads serial input and toggles the relay accordingly |

---

## 🛠️ Troubleshooting

- **WebSocket Issues**:  
  Ensure `server.py` is running and port `8765` is free.
  ```bash
  netstat -a -n -o | find "8765"
  ```

- **MQTT Not Working**:  
  Confirm Mosquitto is running:
  ```bash
  netstat -a -n -o | find "1883"
  ```

- **Serial Issues**:  
  Check the correct port is used in `subscriber.py`.

- **UI Not Displaying Correctly**:  
  Open browser console (F12) to check errors.

---

## 📝 Notes

- Light-mode UI with green styling for clarity.
- Relay assumed to be **active-low**. Adjust in `light.ino` if otherwise.
- Use TLS and authentication for production deployments.
- Update `ws://localhost:8765` in `script.js` if using a custom WebSocket port.

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).

> Made with ❤️ by **Dushimire Aine**

---

