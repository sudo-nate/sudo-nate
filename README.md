
# Nate · ESP32 embedded hardware

I build small ESP32 gadgets for the home — weather displays, clocks, bike-share boards — mostly ESPHome-based, documented as I go.

## IdeaSpark 1.9" ST7789 board: documented findings

- The third onboard button (KEY21) is wired to GPIO4, which doubles as the ST7789 display's hardware reset line — pressing it white-screens the panel with no software recovery path ([full writeup](https://gist.github.com/sudo-nate/c50d2bed5eaa9953e64682bbebf86ad6)).
- Workaround: watch GPIO4 as a button, persist an orientation flag across reboot, and let the reboot reinitialize the panel — turning the flaw into a working 180° rotation toggle, verified on hardware.
- Use the BOOT button (GPIO0) for UI input instead — a real GPIO with no shared hardware function.
- Confirmed pinout (1.9" 320×170 ST7789): MOSI 23, SCLK 18, CS 15, DC 2, RST 4, backlight 32.
- The USB-C port needs a USB-A→USB-C cable (no CC-line handling); OTA is unreliable, so flash over USB.
- My 2-star Amazon review documenting the KEY21 flaw was rejected; the vendor has not acknowledged it.
- The board itself works — but the missing documentation makes it hard to recommend to beginners.

## Three-face ESPHome dashboard (running on this board)

- **Weather** — Portland conditions via OpenWeatherMap, UV index via Open-Meteo, time via NTP.
- **Split-flap clock** — flip-clock face with the date up top.
- **Biketown** — live bike counts for two Portland docks (NE Holladay at 2nd, NE Multnomah at Grand) through a lightweight Cloudflare Worker proxy, refreshing every few minutes.

## Elsewhere

- [espcontrol](https://github.com/sudo-nate/espcontrol) — ESPHome-based smart home control panel (fork, in progress).
