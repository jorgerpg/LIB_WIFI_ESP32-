# Wi-Fi Library for ESP32

## Overview

This library provides a simple interface to initialize and manage a Wi-Fi connection in **station mode (STA)** for the ESP32 microcontroller using the ESP-IDF framework. It handles Wi-Fi events, connects to a specified access point, and manages reconnection in case of disconnection.

---

## Features

- **Automatic Reconnection:** Automatically reconnects to the access point if the connection is lost.
- **Event Handling:** Uses ESP-IDF's event handling to manage Wi-Fi and IP-related events.
- **Thread-Safe Event Groups:** Synchronizes connection status using an event group.
- **Support for WPA2 Security:** Configured to use WPA2-PSK for secure connections.

---

## Requirements

- **ESP-IDF Framework:** Ensure you have ESP-IDF installed and set up in your development environment.
- **Hardware:** ESP32 microcontroller.
- **NVS Flash:** Persistent storage for configuration initialization.

---

## Class API

### `WIFI`

This class encapsulates all the Wi-Fi functionalities. 

### Constructor

```cpp
WIFI(const char *ssid, const char *password);
```

- **Parameters:**
  - `ssid` - The SSID of the Wi-Fi network to connect to.
  - `password` - The password for the Wi-Fi network.

---

### Public Methods

#### `void start()`
Initializes the Wi-Fi and starts the connection process.

**Example:**
```cpp
WIFI wifi("YourSSID", "YourPassword");
wifi.start();
```

---

### Private Methods

#### `void event_handler(void *arg, esp_event_base_t event_base, int32_t event_id, void *event_data)`
Handles Wi-Fi and IP-related events.

- **Events:**
  - `WIFI_EVENT_STA_START` - Initiates the connection process.
  - `WIFI_EVENT_STA_DISCONNECTED` - Attempts to reconnect if disconnected.
  - `IP_EVENT_STA_GOT_IP` - Logs the acquired IP address.

---

#### `void wifi_init_sta()`
Initializes the Wi-Fi in station mode and starts the event loop.

---

## Example Usage

```cpp
#include "dev_wifi.hpp"

extern "C" void app_main()
{
    WIFI wifi("YourSSID", "YourPassword");
    wifi.start();
}
```

---

## Log Output Example

When the library is used, you can expect log outputs similar to the following:

1. **Connection Start:**
   ```
   I (1234) wifi: ESP_WIFI_MODE_STA
   ```

2. **Connected to Access Point:**
   ```
   I (4567) wifi: connected to ap SSID:YourSSID password:YourPassword
   ```

3. **IP Address Acquired:**
   ```
   I (7890) wifi: got ip:192.168.1.100
   ```

4. **Disconnection and Reconnection:**
   ```
   E (1234) wifi: UNEXPECTED EVENT
   ```

---

## Error Handling

The library uses `ESP_ERROR_CHECK` to ensure all ESP-IDF function calls succeed. If any initialization or configuration step fails, the program will halt and log the error details.
