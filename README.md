
# 🖥️ U8g2 Fonts + Adafruit SSD1306 on ESP8266 (SPI OLED Clock)

> OLED Clock with UTF-8 Fonts + Heart Animation using U8g2 and Adafruit GFX on ESP8266

![Platform](https://img.shields.io/badge/platform-ESP8266-blue.svg)
![Display](https://img.shields.io/badge/display-SSD1306%20OLED-black)
![SPI](https://img.shields.io/badge/connection-SPI-yellow)
![License](https://img.shields.io/github/license/olikraus/U8g2)

---

## 📦 Libraries Used

### 1. `Adafruit_SSD1306`

**Description:** OLED driver for SSD1306-based displays, supporting SPI and I2C.

**Installation:**
```
Library Manager > Search "Adafruit SSD1306" > Install
```

**SPI Constructor:**
```cpp
Adafruit_SSD1306 display(WIDTH, HEIGHT, MOSI, CLK, DC, RESET, CS);
```

**Parameters:**
- `WIDTH`: OLED width (usually 128)
- `HEIGHT`: OLED height (usually 64)
- `MOSI`: Data pin (D7 / GPIO13)
- `CLK`: Clock pin (D5 / GPIO14)
- `DC`: Data/Command pin
- `RESET`: Reset pin
- `CS`: Chip Select pin

**Key Methods:**
```cpp
display.begin(VCC_SOURCE);
display.clearDisplay();
display.display();
display.drawPixel(x, y, color);
```

---

### 2. `U8g2_for_Adafruit_GFX`

**Description:** Enables UTF-8 fonts from U8g2 with Adafruit GFX-compatible displays.

**Installation:**
```
Library Manager > Search "U8g2 for Adafruit GFX" > Install
```

**Initialization:**
```cpp
U8G2_FOR_ADAFRUIT_GFX u8g2;
u8g2.begin(display);
```

**Key Methods:**
```cpp
u8g2.setFont(u8g2_font_*);
u8g2.setCursor(x, y);
u8g2.print("Text");
u8g2.getUTF8Width("Text");
u8g2.setFontMode(1); // Transparent background
u8g2.setForegroundColor(WHITE);
```

**Font Tips:**
- `*_t`: Unicode support
- `*_tf`: ASCII-only, fast
- `*_2x_t`: Icons (use `"B"` for heart)

**Popular Fonts:**
| Font Name                          | Description                      |
| ---------------------------------- | -------------------------------- |
| `u8g2_font_logisoso20_tf`          | Modern digital numbers           |
| `u8g2_font_helvB08_tf`             | Helvetica Bold, size 8           |
| `u8g2_font_helvB12_tf`             | Helvetica Bold, size 12          |
| `u8g2_font_open_iconic_human_2x_t` | Icons: heart (`"B"`), user, etc. |

🔗 Font list: https://github.com/olikraus/u8g2/wiki/fntlistall

---

### 3. `ESP8266WiFi`

**Description:** Manages WiFi for ESP8266 boards.

**Key Methods:**
```cpp
WiFi.begin(ssid, password);
WiFi.status();
```

**Status Check:**
```cpp
if (WiFi.status() == WL_CONNECTED) { ... }
```

---

### 4. `time.h`

**Description:** Fetches time from NTP servers using ESP8266 built-in support.

**Configuration:**
```cpp
configTime(GMT_OFFSET_SEC, DST_OFFSET_SEC, "pool.ntp.org", "time.nist.gov");
```

**Extracting Time:**
```cpp
time_t now = time(nullptr);
struct tm* t = localtime(&now);

// Example usage:
t->tm_hour;
t->tm_min;
t->tm_sec;
t->tm_wday; // 0 = Sunday
```

---

## 🧪 Use Case: OLED Clock with Pulse Heart

**Features:**
- Fetches current time via NTP
- Weekday on top (small font)
- Time in center (large font)
- "am/pm" in small font
- Animated pulsing heart

**Fonts Used:**
| Section | Font Name                          |
|--------|-------------------------------------|
| Weekday | `u8g2_font_helvB08_tf`             |
| Time    | `u8g2_font_logisoso20_tf`          |
| AM/PM   | `u8g2_font_helvB08_tf`             |
| Label   | `u8g2_font_helvB12_tf`             |
| Heart   | `u8g2_font_open_iconic_human_2x_t` (char `"B"`) |

---

## 📌 SPI Pin Mapping for NodeMCU (ESP8266)

| Function | GPIO | Label |
|----------|------|-------|
| MOSI     | 13   | D7    |
| CLK      | 14   | D5    |
| DC       | 12   | D6    |
| RESET    | 5    | D1    |
| CS       | 15   | D8    |

---

## ⚠️ Troubleshooting Tips

- Display not working?
  - Check **SPI wiring**
  - Verify **3.3V/5V compatibility**
  - Ensure `display.begin()` returns `true`
- Always:
  - Call `display.clearDisplay()` before drawing
  - Call `display.display()` after drawing
- Use `u8g2.getUTF8Width()` to center text precisely

---

## 📎 Useful Links

- 🔠 U8g2 Font List: https://github.com/olikraus/u8g2/wiki/fntlistall
- 📘 Adafruit SSD1306 GitHub: https://github.com/adafruit/Adafruit_SSD1306
- 🧩 U8g2 for Adafruit GFX: https://github.com/olikraus/U8g2_for_Adafruit_GFX
- ⏰ ESP8266 Time Docs: https://arduino-esp8266.readthedocs.io/en/latest/libraries.html#time

---

## 📁 License

This project uses open source libraries under their respective licenses.
