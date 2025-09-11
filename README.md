# Marstek Energy Storage – Home Assistant Integration

IMPORTANT: Please note that I´m not a programmer. This Home Assistant Integration was designed by me just with the help of Chat gpt 5 by Trial and error. I will not support and also not continue to develop this Integration. Understand this Version just as Proof of Concept. Possible that below mentioned sensors, switches, etc,  will not work or be unstable. The Hardware I used to test everyting was my own Marstek Venus v2, Firmware V154 and with enabled API on port 30000.

> **Version:** 0.6.11 · **Domain:** `marstek` · **IoT class:** `local_polling`

A lightweight local integration for Marstek energy storage systems (UDP-JSON).  
It retrieves status data and allows switching between operating modes.

---

## ✨ Features
- **UI auto-discovery** (Config Flow)
- **Sensors** for SoC, battery/PV/grid power, energy counters, and more
- **Control operating mode**: `Auto`, `AI`, `Manual`, `Passive`
- **Number entities** for fine-tuning in *Passive* mode (power, countdown)
- **Binary sensors** for charge/discharge permission
- **Switch/Select entities** for quick mode switching

---

## 📦 Installation

**Manual**
1. Create the folder `custom_components/marstek` in your Home Assistant `config` directory.  
2. Copy the contents of this repository into that folder.  
3. Restart Home Assistant.  

---

## ⚙️ Setup (UI)
**Settings → Devices & Services → Add Integration → “Marstek”**, then:

Required fields:
- **IP** *(Marstek battery)*  
- **Device ID** *(default: `0`)*  
- **Port** *(default: `30000`)*  
- **Scan Interval (s)** *(default: `10`)*  
- **Your local_Homeassistant-IP** and **local Homeassistant Port** for binding the outgoing UDP socket (e.g. under strict NAT)
  (**Note!: The Port you set for the Homeassistant Port must be equal to the above Mastek battery Port, else Homeassistant will not receive any data**) 
- **Timeout (s)** *(default: `5`)*  

---

## 🧩 Entities (Overview)

### Sensors
- **Battery SoC** (`bat_soc`/`soc`)  
- **Battery Power** (`bat_power`), **PV Power** (`pv_power`)  
- **Grid Import/Export Power** (`offgrid_power`/`ongrid_power`)  
- **Total PV/Grid/Load Energy** (`total_*_energy`, scaled, partly ×0.1)  
- **Battery Temperature / Capacity / Rated Capacity**

### Select
- **Operating Mode** with options: `Auto`, `AI`, `Manual`, `Passive`

### Numbers (only valid in *Passive* mode)
- **Passive Power** *(W, 0–10000)*  
- **Passive Countdown** *(s, 0–86400)*  

### Binary Sensors
- **Battery Charging Allowed**  
- **Battery Discharging Allowed**

### Switches
- Quick selectors for modes (`Auto`, `AI`, `Passive`).  

### Service
- **`marstek.refresh_now`** – forces an immediate data refresh.  

---

## 🔌 Network & Protocol
- Communication via **UDP** directly to the storage system’s IP (`Port 30000` by default).  
- Optional **local binding** (`local_IP`/`local_port`) for controlled source ports.  
- Configurable timeouts & retries, smoothing for small power fluctuations.  

---

## 🛠 Troubleshooting
- **No data?** Check IP/port/device ID and confirm the device is reachable in the same network.  
- **Strict NAT/firewall?** Set `local_IP`/`local_port` and allow inbound responses.  
- **Strange fluctuations?** Increase `Min power delta (W)`.  
- **Need `unavailable` on timeout?** Enable *Fail unavailable on timeout*.  
- **Enable logs:** add `logger:` with `custom_components.marstek: debug`.  

---

## 📁 Project Structure (Short)
```
custom_components/marstek/
├── __init__.py            # Setup, coordinator, service
├── api.py                 # UDP client (ES.GetStatus / ES.GetMode / ES.SetMode / Bat.GetStatus)
├── coordinator.py         # DataUpdateCoordinator with retry/smoothing
├── sensor.py              # Dynamic + fixed battery sensors
├── switch.py              # Mode switches
├── number.py              # Passive power/countdown
├── select.py              # Operating mode
├── binary_sensor.py       # Charge/Discharge allowed
├── manifest.json          # domain, name, version, iot_class, config_flow
├── strings.json, translations/
└── diagnostics.py
```

---

## 📜 License & Maintainer
- **Codeowners:** `lemuba`

---

**Good luck!** 🚀  
