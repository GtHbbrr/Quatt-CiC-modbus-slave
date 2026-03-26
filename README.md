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
| **GPIO08** | **TXD / DI / RX-I** | Modbus TX |
| **GPIO09** | **RXD / RO / TX-O** | Modbus RX |
| **GPIO10** | **DE / RE / RTS** | Flow Control (Half-duplex) |

> **Tip:** Gebruik een getwist aderpaar (bijv. uit een Cat5e kabel) voor de **A+** en **B-** verbinding naar de Quatt.

---

## 📊 Belangrijke Registers

De configuratie dekt de registers voor de Quatt CiC:


| Register | Sensor name                  | Register | Sensor name                  |
| :---     | :--------------------------- | :---     | :--------------------------- |
| R3999    | Working Mode set by CiC      | R2010    | Pump Mode set by CiC         |
| R1999    | Compressor Level set by CiC  | R2015    | Pump Level set by CiC        |
| R2006    | Silent mode set by CiC       | R2116    | Evaporator Pressure          |
| R2099    | Working Mode Actual          | R2117    | Condenser Pressure           |
| R2100    | Compressor AC Voltage        | R2118b0  | Defrost Mode                 |
| R2101    | Compressor AC Current        | R2119b0  | Alarm - Main Line Current    |
| R2102    | Compressor Frequency Demand  | R2119b3  | Info - Compressor Oil Return |
| R2103    | Compressor Frequency Actual  | R2119b4  | Alarm - High Pressure Switch |
| R2104    | Fan Speed Maximum            | R2119b6  | Alarm - 1st Start Pre-heat   |
| R2105    | Fan Speed Actual             | R2119b9  | Alarm - AC High/Low Voltage  |
| R2107    | Electric Expansion Valve     | R2119b12 | Alarm - Low Pressure Switch  |
| R2108b2  | Bottom Heater                | R2119    | Status bits R2119            |
| R2108b3  | Crankcase Heater             | R2120    | Status bits R2120            |
| R2108b0  | Fan Low Speed Mode           | R2121    | Status bits R2121            |
| R2108b4  | Fan Defrost Speed Mode       | R2122    | Firmware Version             |
| R2108b5  | Fan High Speed Mode          | R2123    | EEPROM Version               |
| R2108b6  | 4way Valve                   | R2127    | Main control board item No   |
| R2108b11 | Pump Relay                   | R2131    | Condensing Temperature       |
| R2108bx  | Other bits R2108             | R2132    | Evaporating Temperature      |
| R2110    | Outside Temperature          | R2133    | Water In Temperature         |
| R2111    | Evaporator Coil Temperature  | R2134    | Water Out Temperature        |
| R2112    | Gas Discharge Temperature    | R2135    | Condenser Coil Temperature   |
| R2113    | Gas Return Temperature       | R2137    | Pump Power                   |
| R2115    | DIPswitch readout            | R2138    | Pump Flow                    |



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

