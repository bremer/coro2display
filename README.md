# CorO<sub>2</sub>Sens

Build a simple device that warns if CO<sub>2</sub> concentration in a room becomes a risk for Covid-19 aerosol infections.

- Measures CO<sub>2</sub> concentration in room air.
- Controls an RGB LED (green, yellow, red, like a traffic light).
- A buzzer can be connected that alarms if levels are critical.

This project was heavily inspired by [ideas from Umwelt-Campus Birkenfeld](https://www.umwelt-campus.de/forschung/projekte/iot-werkstatt/ideen-zur-corona-krise).

You can also find a good overview of the topic by Rainer Winkler here: [Recommendations for use of CO2 sensors to control room air quality during the COVID-19 pandemic](https://medium.com/@rainer.winkler.poaceae/recommendations-for-use-of-co2-sensors-to-control-room-air-quality-during-the-covid-19-pandemic-c04cac6644d0).
(Use his code if you have MH-Z19B sensors.)


## Sensors
- The sensor used here is the Sensirion SCD30 (around USD 50 / 40 €) which is optionally augmented by a BME280 pressure sensor to improve accuracy.
- [Look here](https://github.com/RainerWinkler/CO2-Measurement-simple) if you want to use MH-Z19B sensors.
 

## Threshold values
| LED color                 |CO<sub>2</sub> concentration |
|:--------------------------|:----------------------------|
| Green ("all good")        | < 800 ppm                  |
| Yellow ("open windows")   | 800 – 1000 ppm             |
| Red ("leave room")        | \> 1000 ppm                 |

Based on a [Recommendation from the REHVA](https://www.rehva.eu/fileadmin/user_upload/REHVA_COVID-19_guidance_document_V3_03082020.pdf)
(Federation of European Heating, Ventilation and Air Conditioning associations, [rehva.eu](https://www.rehva.eu/).
for preventing COVID-19 aerosol spread, especially in schools. 


## Web server
You can read current levels and a simple graph for the last hour by connecting to the WiFi `coro2sens` that is created.
Most devices will open a captive portal, immediately showing the data. You can also open `http://10.0.0.1/` in a browser.


## You need
1. Any ESP32 or ESP8266 board (like a [WEMOS D32](https://docs.wemos.cc/en/latest/d32/d32.html) (about $18 / 15€) or [WEMOS LOLIN D1 Mini](https://docs.wemos.cc/en/latest/d1/d1_mini.html) (about $7 / 6€)).
  - ESP32 has bluetooth, for future expansion.
1. [Sensirion SCD30](https://www.sensirion.com/en/environmental-sensors/carbon-dioxide-sensors/carbon-dioxide-sensors-co2/) carbon dioxide sensor module ([mouser.com](https://mouser.com/ProductDetail/Sensirion/SCD30?qs=rrS6PyfT74fdywu4FxpYjQ==)).
1. 1 NeoPixel compatible RGB LED (WS2812B).
1. Optional: BME280 I<sup>2</sup>C pressure sensor module, improves accuracy (less than $5 / 4€).   
1. Optional: 3V piezo buzzer or simple speaker.
1. A nice case :)


### Wiring

| ESP pin      | goes to                                    |
|:-------------|:-------------------------------------------|
| 3V3          | SCD30 VIN, BME280 VIN                      |
| 5V           | LED +5V                                    |
| GND          | SCD30 GND, BME280 GND, LED GND, Buzzer (-) |
| SCL / D1     | SCD30 SCL, BME280 SCL                      |
| SDA / D2     | SCD30 SDA, BME280 SDA                      |
| GPIO 0 / D3  | LED DIN                                    |
| GPIO 14 / D5 | Buzzer (+)                                 |

(GPIOs can easily be changed in `src/main.cpp`)


### Flashing the ESP using PlatfomIO
- Simply open the project, select your env (ESP8266 / ESP32) and run / upload.
- Or via command line:
  - `platformio run -t -e esp21e upload` for ESP8266.
  - `platformio run -t -e esp32dev upload` for ESP32.

### Flash using the Arduino IDE
- Open `coro2sense.ino` in the `coro2sens` sub folder in your Arduino IDE.
- Install (or update) your board platform (*Tools –> Board –> Board Manager...*):
  - Install `esp8266` or `esp32`.
- Install (or update) the following libraries using the built-in library manager (*Tools –> Library Manager...*)
  - **For ESP8266:**
    - `SparkFun BME280`
    - `Adafruit NeoPixel`
  - **For ESP32:**
    - `SparkFun SCD30 Arduino Library`
    - `SparkFun BME280`
    - `Adafruit NeoPixel`
- Install the following external libraries (download .zip file, then import it via *Sketch –> Include Library –> Add .ZIP Library...*):
  - **For ESP8266:**
    - [paulvha/scd30](https://github.com/paulvha/scd30) ([.zip](https://github.com/paulvha/scd30/archive/master.zip))
    - [me-no-dev/ESPAsyncTCP](https://github.com/me-no-dev/ESPAsyncTCP) ([.zip](https://github.com/me-no-dev/ESPAsyncTCP/archive/master.zip))
    - [me-no-dev/ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer) ([.zip](https://github.com/me-no-dev/ESPAsyncWebServer/archive/master.zip))
  - **For ESP32:**
    - [lbernstone/Tone32](https://github.com/lbernstone/Tone32) ([.zip](https://github.com/lbernstone/Tone32/archive/master.zip))
    - [me-no-dev/AsyncTCP](https://github.com/me-no-dev/AsyncTCP) ([.zip](https://github.com/me-no-dev/AsyncTCP/archive/master.zip))
    - [me-no-dev/ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer) ([.zip](https://github.com/me-no-dev/ESPAsyncWebServer/archive/master.zip))
- Run & upload :)


Please let me know of any issues you might encounter ([open a GitHub issue](https://github.com/kmetz/coro2sens/issues/new/choose) or write me on [twitter.com/kmetz](https://twitter.com/kmetz) or k@kjpm.de).
