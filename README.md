
# 🌤️ ESP8266 OLED Weather & Clock Display

A stylish microproject using **NodeMCU ESP8266**, **128x64 SPI OLED**, and **OpenWeatherMap API**. Displays live **temperature**, **weather icons**, **real-time clock**, and a cute **animated heart pulse**.

![ESP8266 OLED Weather Display](https://your-image-link-here.png) <!-- optional screenshot -->

---

## 📦 Features

- ✅ Real-time weather via OpenWeatherMap API
- 🕒 NTP-synced digital clock
- 🌡️ Temperature + Weather icons
- 💓 Animated heart pulse effect
- 📶 Auto WiFi reconnect
- 🔄 API key rotation for rate-limit safety

---

## 🛠️ Hardware Requirements

| Component       | Description              |
|----------------|--------------------------|
| ESP8266 Board  | NodeMCU, Wemos, etc.     |
| OLED Display   | 128x64 SPI SSD1306       |
| Power Supply   | 5V Micro USB / 3.3V      |

---

## 📚 Libraries Used

```cpp
#include <ESP8266WiFi.h>
#include <time.h>
#include <Adafruit_SSD1306.h>
#include <U8g2_for_Adafruit_GFX.h>
#include <ESP8266HTTPClient.h>
#include <ArduinoJson.h>
#include <math.h>
```

Install these via **Library Manager** or PlatformIO.

---

## 📡 WiFi & API Setup

```cpp
const char* ssid = "your_wifi_name";
const char* password = "your_wifi_password";

const char* city = "Kochi";
const char* countryCode = "IN";

const char* apiKeys[] = {
  "API_KEY_1", "API_KEY_2", "API_KEY_3" // use multiple for load distribution
};
```

- **API Source**: [OpenWeatherMap](https://openweathermap.org/api)
- Set units to metric (°C)

---

## ⏰ Time Synchronization (NTP)

```cpp
configTime(19800, 0, "pool.ntp.org", "time.nist.gov");
```

- IST (GMT +5:30) used
- Retries for up to 10 seconds if unsynced

---

## 🖥️ OLED SPI Pinout (NodeMCU)

| Signal  | OLED Pin | ESP8266 Pin |
|---------|----------|-------------|
| MOSI    | D1       | GPIO13      |
| CLK     | D0       | GPIO14      |
| DC      | D2       | GPIO12      |
| RESET   | D3       | GPIO5       |
| CS      | D8       | GPIO15      |

> 📐 Display Resolution: `128x64`

---

## 🎨 Fonts & Icons

| Element         | Font                            |
|----------------|----------------------------------|
| Regular Text    | `u8g2_font_helvR08_tf`          |
| Time Display    | `u8g2_font_logisoso20_tf`       |
| Weather Icons   | `u8g2_font_open_iconic_weather_1x_t` |
| Heart Icon      | `u8g2_font_open_iconic_human_2x_t`   |

---

<details>
<summary>💓 Heart Animation Logic</summary>

```cpp
float pulseAngle = 0;
const float PULSE_SPEED = 0.15;
const int PULSE_AMPLITUDE = 2;

int offset = (int)(sin(pulseAngle) * PULSE_AMPLITUDE);
pulseAngle += PULSE_SPEED;
```

- Smooth up/down bounce
- Drawn alongside "makrii" branding
</details>

---

<details>
<summary>🌥️ Weather Fetching Code</summary>

```cpp
void fetchWeather() {
  HTTPClient http;
  String url = String("http://api.openweathermap.org/data/2.5/weather?q=") +
               city + "," + countryCode + "&appid=" + apiKeys[apiKeyIndex] + "&units=metric";

  http.begin(url);
  int httpCode = http.GET();

  if (httpCode == HTTP_CODE_OK) {
    String payload = http.getString();
    DynamicJsonDocument doc(1024);
    deserializeJson(doc, payload);

    weatherMain = doc["weather"][0]["main"].as<String>();
    temperature = doc["main"]["temp"].as<float>();
  }

  apiKeyIndex = (apiKeyIndex + 1) % NUM_API_KEYS;
}
```

- Rotates API keys
- Parses JSON via `ArduinoJson`
</details>

---

## 🔁 Main Loop Breakdown

```cpp
void loop() {
  if (WiFi.status() != WL_CONNECTED) {
    connectToWiFi();  // Handles reconnect
  }

  unsigned long currentMillis = millis();
  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;
    fetchWeather();
  }

  displayTimeWeather();  // Always running
}
```

---

## 📊 OLED Display Layout

```text
┌─────────────────────────────┐
│      Mon | ☁️ 29°C | Clouds │
│        10:25:42 AM          │
│         makrii ❤️           │
└─────────────────────────────┘


```

---

## ⚠️ Error Handling

- WiFi reconnect loop
- NTP sync timeout message
- Skips weather update if offline
- Continues display even if fetch fails

---

## 🧠 TODO / Enhancements

- [ ] 12/24 hour toggle
- [ ] OLED brightness dimming at night
- [ ] Add humidity and wind info
- [ ] Touch input / button control

---

## 📝 License & Attribution

MIT License  
Developed by [makrii]  
Fonts from [U8g2 Iconic Fonts](https://github.com/olikraus/u8g2/wiki/fntgrpiconic)

---

## 📁 File Info

- **Platform**: ESP8266 (NodeMCU)
- **Filename**: `weather_clock_esp8266.ino`
- **Display**: SSD1306 OLED 128x64 (SPI)
- **Tested On**: Arduino IDE v2.2

---

⭐ *If you found this helpful, give the project a star!*
