# Quatt Modbus Slave (HP1 Emulator) for ESPHome

Deze repository bevat de ESPHome-configuratie voor een **ESP32-S3 Super Mini** die fungeert als een **Modbus Slave (Server)**. Hiermee emuleer je de Heat Pump (HP1) unit om te communiceren met de **Quatt CiC (Master)**.

Deze versie is volledig bijgewerkt voor compatibiliteit met de nieuwste Quatt-firmware (**v2.20.2 en hoger**).

---

## 🚀 Kenmerken
* **Geoptimaliseerd voor CiC v2.20+**: Aangepaste timing en register-mapping.
* **Snelle Respons**: `send_wait_time` op **50ms** om timeouts te voorkomen.
* **Volledige Register Map**: Inclusief temperaturen, drukwaarden, EEV-status en werkmodi.
* **Live Debugging**: Ingebouwde UART-sniffer om Modbus-verkeer in de logs te monitoren.
* **Storingsvrij**: Logger-baudrate op 0 om interferentie met de RS485-bus te voorkomen.

---

## 🔌 Aansluitschema (Hardware)

Gebruik een **RS485-naar-TTL converter** om de ESP32-S3 te verbinden met de Quatt CiC.


| ESP32-S3 Super Mini Pin | Spark Fun RS485breakout Module Pin | Functie |
| :--- | :--- | :--- |
| **5V / VIN** | **VCC** | Voeding (5V) |
| **GND** | **GND** | Ground |
| **GPIO08** | **TXD / DI** | Modbus TX |
| **GPIO09** | **RXD / RO** | Modbus RX |
| **GPIO10** | **DE / RE** | Flow Control (Half-duplex) |

> **Tip:** Gebruik een getwist aderpaar (bijv. uit een Cat5e kabel) voor de **A+** en **B-** verbinding naar de Quatt.

---

## 📊 Belangrijke Registers

De configuratie dekt de meest kritieke registers voor de Quatt CiC:


| Register | Adres (Hex) | Beschrijving |
| :--- | :--- | :--- |
| **R1999** | `0x07cf` | Compressor Level Demand (Writable) |
| **R2110** | `0x083e` | Buitentemperatuur (Outside Temp) |
| **R2118** | `0x0846` | Status bits (**Bit 0: Defrost Mode**) |
| **R2119** | `0x0847` | Alarm bits (High/Low pressure, Voltage) |
| **R2134** | `0x0856` | Water Out Temperature |

---

## 🛠 Installatie
1. Kopieer de inhoud van de YAML naar je ESPHome dashboard.
2. Pas indien nodig de `wifi` en `api` secties aan.
3. Flash de code naar je ESP32-S3.
4. Controleer de logs voor `[uart_debug]` om te zien of de CiC gegevens opvraagt.

---

## 🔍 Troubleshooting
* **Geen data in de logs?** Wissel de A+ en B- draden om op de RS485 module.
* **CRC Errors?** Controleer of de `parity` op `EVEN` staat en of je een 120 Ohm terminatieweerstand gebruikt.
* **Timeouts?** Verlaag de `send_wait_time` in de YAML naar `20ms`.

---
*Disclaimer: Dit project is niet gelieerd aan Quatt. Gebruik op eigen risico.*

