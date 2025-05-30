# U8g2\_for\_Adafruit\_GFX with Adafruit\_SSD1306 on ESP8266 (SPI) - Detailed Documentation

## 📦 Libraries Used

### 1. `Adafruit_SSD1306.h`

#### Description:

This library drives OLED displays based on the SSD1306 controller. It supports both I2C and SPI modes.

#### Installation:

```
Library Manager > Search "Adafruit SSD1306" > Install
```

#### Constructor for SPI:

```cpp
Adafruit_SSD1306 display(WIDTH, HEIGHT, MOSI, CLK, DC, RESET, CS);
```

##### Parameters:

* `WIDTH`: Width of the screen (usually 128)
* `HEIGHT`: Height of the screen (usually 64)
* `MOSI`: Data pin (D7 / GPIO13)
* `CLK`: Clock pin (D5 / GPIO14)
* `DC`: Data/Command pin
* `RESET`: Reset pin
* `CS`: Chip Select pin

#### Important Methods:

* `begin(VCC_SOURCE)`: Initializes the display
* `clearDisplay()`: Clears the internal buffer
* `display()`: Writes the buffer to the screen
* `drawPixel(x, y, color)`: Draws a pixel

---

### 2. `U8g2_for_Adafruit_GFX.h`

#### Description:

This library bridges U8g2 fonts (extensive UTF-8 and icon fonts) with any Adafruit GFX-compatible display like SSD1306.

#### Installation:

```
Library Manager > Search "U8g2 for Adafruit GFX" > Install
```

#### Initialization:

```cpp
U8G2_FOR_ADAFRUIT_GFX u8g2;
u8g2.begin(display);
```

#### Key Methods:

* `setFont(u8g2_font_*)`: Set current font
* `setCursor(x, y)`: Set cursor for text
* `print(text)`: Print UTF-8 text
* `getUTF8Width(text)`: Get width of text (for centering)
* `setFontMode(1)`: Enable transparent background
* `setForegroundColor(WHITE)`: Set text color (optional, needed for OLED)

#### Font Naming Convention:

* `*_t`: Unicode version
* `*_tf`: ASCII-only fast fonts
* `*_2x_t`: Large icon fonts

#### Popular Fonts:

| Font Name                          | Description                            |
| ---------------------------------- | -------------------------------------- |
| `u8g2_font_logisoso20_tf`          | Large digital/modern numbers           |
| `u8g2_font_helvB08_tf`             | Helvetica Bold, size 8                 |
| `u8g2_font_helvB12_tf`             | Helvetica Bold, size 12                |
| `u8g2_font_open_iconic_human_2x_t` | Heart, user, icons (use "B" for heart) |

📌 Font list preview: [https://github.com/olikraus/u8g2/wiki/fntlistall](https://github.com/olikraus/u8g2/wiki/fntlistall)

---

### 3. `ESP8266WiFi.h`

#### Description:

Handles WiFi connection for ESP8266.

#### Key Methods:

* `WiFi.begin(ssid, password)`: Starts WiFi connection
* `WiFi.status()`: Get current status
* `WL_CONNECTED`: Constant used to check if connected

---

### 4. `time.h`

#### Description:

Fetches time from NTP servers using ESP8266’s built-in time support.

#### Configuration:

```cpp
configTime(GMT_OFFSET_SEC, DST_OFFSET_SEC, "pool.ntp.org", "time.nist.gov");
```

* `GMT_OFFSET_SEC`: e.g., 19800 for GMT+5:30 (India)
* `DST_OFFSET_SEC`: 0 or 3600 depending on daylight saving

#### Time Extraction:

```cpp
time_t now = time(nullptr);
struct tm* t = localtime(&now);
```

* `t->tm_hour`, `t->tm_min`, `t->tm_sec`: Time parts
* `t->tm_wday`: Day of the week (0 = Sunday)

---

## 🧪 Use Case Example: OLED Clock with Pulse Heart

* Displays current time from NTP (12h format)
* Weekday in small font on top
* Time in large centered font
* "am/pm" label in small font
* Animated heart icon pulsing

### Fonts Used:

| Purpose | Font                                           |
| ------- | ---------------------------------------------- |
| Weekday | `u8g2_font_helvB08_tf`                         |
| Time    | `u8g2_font_logisoso20_tf`                      |
| am/pm   | `u8g2_font_helvB08_tf`                         |
| Name    | `u8g2_font_helvB12_tf`                         |
| Heart   | `u8g2_font_open_iconic_human_2x_t` (use `"B"`) |

### SPI Pin Mapping for NodeMCU (ESP8266):

| Function | GPIO | Label |
| -------- | ---- | ----- |
| MOSI     | 13   | D7    |
| CLK      | 14   | D5    |
| DC       | 12   | D6    |
| RESET    | 5    | D1    |
| CS       | 15   | D8    |

---

## ⚠️ Tips & Troubleshooting

* If display shows nothing, check:

  * SPI wiring
  * Display voltage (3.3V vs 5V)
  * Initialization success (`display.begin()` must return true)
* Use `display.clearDisplay()` before every draw cycle.
* Always call `display.display()` after drawing.
* Use `getUTF8Width()` to center text.

---

## 📎 Useful Links

* U8g2 Font List: [https://github.com/olikraus/u8g2/wiki/fntlistall](https://github.com/olikraus/u8g2/wiki/fntlistall)
* Adafruit SSD1306 GitHub: [https://github.com/adafruit/Adafruit\_SSD1306](https://github.com/adafruit/Adafruit_SSD1306)
* U8g2 for Adafruit GFX GitHub: [https://github.com/olikraus/U8g2\_for\_Adafruit\_GFX](https://github.com/olikraus/U8g2_for_Adafruit_GFX)
* ESP8266 Arduino Core Time: [https://arduino-esp8266.readthedocs.io/en/latest/libraries.html#time](https://arduino-esp8266.readthedocs.io/en/latest/libraries.html#time)

---
