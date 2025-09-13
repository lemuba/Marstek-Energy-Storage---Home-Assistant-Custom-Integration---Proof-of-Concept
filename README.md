# Marstek Energy Storage -- Home Assistant Custom Integration (Proof of Concept)

IMPORTANT: Please note that I´m not a programmer. This Home Assistant
Integration was created with the help of ChatGPT by trial and error.\
This is only a **Proof of Concept**. I may not provide support or
further development.\
Some sensors, switches, or features may not work reliably.

Tested with: **Marstek Venus v2** · **Firmware V154** · **API enabled on
port 30000**

> **Version:** 0.6.11 · **Domain:** `marstek` · **IoT class:**
> `local_polling`

A lightweight local integration for Marstek energy storage systems
(UDP-JSON).\
It retrieves status data and allows switching between operating modes.

------------------------------------------------------------------------

## 📂 Project Structure

The integration must be installed under the Home Assistant
`custom_components/` directory:

    custom_components/marstek/
    ├── __init__.py
    ├── api.py
    ├── coordinator.py
    ├── sensor.py
    ├── switch.py
    ├── number.py
    ├── select.py
    ├── binary_sensor.py
    ├── manifest.json
    ├── diagnostics.py
    ├── strings.json
    ├── translations/
    │   └── en.json

👉 The folder name must be exactly `marstek`, otherwise Home Assistant
will not detect the integration.

------------------------------------------------------------------------

## ✨ Features

-   **UI auto-discovery** (Config Flow)
-   **Sensors**: SoC, battery/PV/grid power, energy counters, etc.
-   **Control operating mode**: `Auto`, `AI`, `Manual`, `Passive`
-   **Number entities** for Passive mode (power, countdown)
-   **Binary sensors** for charge/discharge permission
-   **Switch/Select entities** for quick mode switching
-   **Service**: `marstek.refresh_now` to force an update

------------------------------------------------------------------------

## 📦 Installation

1.  Create the folder `custom_components/marstek` in your Home Assistant
    `config` directory.\
2.  Copy all contents of this repository into that folder.\
3.  Restart Home Assistant.

------------------------------------------------------------------------

## ⚙️ Setup (UI)

Go to: **Settings → Devices & Services → Add Integration → "Marstek"**,
then enter:

-   **IP** (Marstek battery)\
-   **Device ID** (default: `0`)\
-   **Port** (default: `30000`)\
-   **Scan Interval (s)** (default: `10`)\
-   **Local Home Assistant IP + Port** (required under strict NAT)\
-   **Timeout (s)** (default: `5`)

⚠️ Note: The Home Assistant port must match the Marstek port, otherwise
no data will be received.

------------------------------------------------------------------------

## 🧩 Entities

### Sensors

-   Battery SoC (`bat_soc`)
-   Battery Power, PV Power, Grid Import/Export Power
-   Energy counters (PV, grid, load, battery)
-   Battery temperature, capacity, rated capacity

### Select

-   Operating mode (`Auto`, `AI`, `Manual`, `Passive`)

### Numbers (Passive mode only)

-   Passive Power (0--10000 W)\
-   Passive Countdown (0--86400 s)

### Binary Sensors

-   Battery Charging Allowed\
-   Battery Discharging Allowed

### Switches

-   Quick selectors for modes (`Auto`, `AI`, `Passive`)

------------------------------------------------------------------------

## 🔌 Network & Protocol

-   Uses **UDP** to communicate with the battery (default port 30000)\
-   Optional binding to `local_IP` and `local_port` for NAT/firewall
    setups\
-   Configurable retries and smoothing

------------------------------------------------------------------------

## 🛠 Troubleshooting

-   **No data?** Check IP/port/device ID and ensure the device is
    reachable\

-   **Strict NAT/firewall?** Configure `local_IP`/`local_port`\

-   **Fluctuations?** Increase *Min power delta (W)*\

-   **Need `unavailable` on timeout?** Enable *Fail unavailable on
    timeout*\

-   **Debug logs:**

    ``` yaml
    logger:
      default: warning
      logs:
        custom_components.marstek: debug
    ```

------------------------------------------------------------------------

## 📜 License & Maintainer

-   **Codeowners:** `lemuba`\
-   MIT License

------------------------------------------------------------------------

**Good luck & have fun!** 🚀
