<h1 align="center">Creality Plotter Software</h1>

<p align="center">
  Turn your <b>Creality Ender 3 V3 SE</b> into a pen plotter.<br>
  Plug in with USB-C, load an image, convert it to SVG, and watch your printer draw it.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-macOS%20%7C%20Windows-blue" alt="Platform">
  <img src="https://img.shields.io/badge/built%20with-Electron-47848F" alt="Electron">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
  <img src="https://img.shields.io/badge/status-in%20development-orange" alt="Status">
</p>

---

## What is this?

A 3D printer is already a precise motion machine. Swap the hot end for a pen holder and it becomes a pen plotter.

**Creality Plotter Software** is the missing piece: a desktop app that talks to your printer over USB, turns images into plottable SVG paths, converts those paths to G-code, and streams them to the printer while you watch a live preview.

It is built around the **Creality Ender 3 V3 SE** because its **USB-C port** lets a computer talk to it directly.

> **Status:** early development. The [Roadmap](#roadmap) shows what works and what is planned.

## Features

- **Direct USB-C connection** to the Ender 3 V3 SE on macOS and Windows
- **Automatic serial port detection**, with a dropdown if several devices are found
- **Image to SVG**: load a PNG or JPG and get clean vector paths that work with any plotter
- **SVG to G-code** with pen up/down control, speed settings, and bed-size limits so the toolhead never leaves the bed
- **Live preview** before and during plotting
- **Start, pause, resume, and stop** controls
- **Pen calibration helper** for pen-down and pen-up heights
- **Handwriting scan (Pro)**: turn your own handwriting into plottable paths
- **Open source**, built with Electron and Node.js

## Free vs Pro

| Feature | Free | Pro |
|---|:---:|:---:|
| USB-C connection to Ender 3 V3 SE | Yes | Yes |
| Image to SVG | Yes | Yes |
| SVG to G-code and plotting | Yes | Yes |
| Live preview and controls | Yes | Yes |
| Handwriting scan | No | Yes |
| Handwriting to plottable font | No | Yes |

The free version is fully usable for plotting. Pro supports continued development.

## What you need

- Creality Ender 3 V3 SE
- A **USB-C cable that supports data** (some cheap cables only charge)
- A Mac or Windows computer
- A pen and the printed pen holder (see below)
- Paper, plus masking tape or binder clips to hold it flat
- Node.js 18 or newer if you run from source

## Pen holder

**Model:** [Pen Plotter for Ender 3 V3 SE on Printables](https://www.printables.com/model/681121-pen-plotter-for-ender-3-v3-se)

Download the files from the model's [Files tab](https://www.printables.com/model/681121-pen-plotter-for-ender-3-v3-se/files), print them, and follow the author's assembly notes.

Tips:

- Keep clips and tape out of the drawing area so nothing crashes into the toolhead.
- Use a firm-tipped pen like a fineliner or gel pen. Soft felt tips wear fast.
- A little spring give in the holder makes lines much more consistent.

## Hardware setup

1. Power off the printer.
2. Attach the pen holder to the toolhead using the model's instructions.
3. Insert the pen so the tip sits slightly below the lowest point of the toolhead.
4. Tape or clip a sheet of paper flat on the bed.
5. Connect the USB-C cable from the printer to your computer.
6. Power on the printer.
7. Home the printer from its own screen so the axes are in a known position.

## Install

### Option A: download a release (easiest)

1. Open the [Releases](../../releases) page of this repo.
2. Download the file for your system: the `.exe` installer for Windows or the `.dmg` for macOS.
3. Install and open it.

On macOS, if the app is not code-signed, right-click it and choose **Open** the first time.

### Option B: run from source

```bash
git clone https://github.com/YOUR-USERNAME/creality-plotter-software.git
cd creality-plotter-software
npm install
npm start
```

### USB drivers

- **macOS:** usually works with no extra driver.
- **Windows:** Windows 10 and 11 normally install the driver on their own. If the printer does not show up as a COM port, install the CH340 serial driver and reconnect.

## Quick start

1. Open the app.
2. Pick your serial port and click **Connect**. Default baud rate is `115200`.
3. Click **Load Image** and choose a PNG or JPG.
4. Adjust the conversion sliders until the SVG preview looks right.
5. Set the size and position of the drawing on the 220 x 220 mm bed.
6. Open **Pen Calibration** and set the pen-down and pen-up heights.
7. Click **Preview** to see the exact path the pen will follow.
8. Click **Start Plot**.

You can pause, resume, or stop at any time.

## Calibration

Pen height is the most important setting.

1. Home the printer.
2. Open **Pen Calibration** in the app.
3. Lower Z in 0.1 mm steps until the pen just touches the paper. Save it as **Pen Down Z**.
4. Raise Z until the pen clears the paper by 2 to 3 mm. Save it as **Pen Up Z**.
5. Click **Draw Test Square** and check that the lines are clean and the square closes.

Faint lines mean Pen Down Z is too high. A dragging pen or torn paper means it is too low.

## How it works

1. **Image to SVG:** the image is traced into vector paths.
2. **Path optimization:** paths are reordered to cut down on pen-up travel.
3. **SVG to G-code:** every path becomes moves, with pen up and pen down as Z moves.
4. **Bed clamping:** coordinates are scaled and limited to the 220 x 220 mm area.
5. **Streaming:** G-code is sent line by line, and the app waits for the printer to reply `ok` before sending the next line.

Example output:

```gcode
G21            ; millimeters
G90            ; absolute positioning
G28            ; home all axes
G0 Z5 F1000    ; pen up
G0 X50 Y50 F6000
G1 Z0 F500     ; pen down
G1 X100 Y50 F2000
G1 X100 Y100
G0 Z5 F1000    ; pen up
```

The nozzle and bed heaters stay off the whole time.

## Handwriting scan (Pro)

1. Write on plain white paper with a dark pen.
2. Take a clear, flat, well-lit photo or scan.
3. Import it in the **Handwriting** tab.
4. The app cleans the image, finds the strokes, and turns them into single-line paths.
5. Plot it directly or export it as SVG.

Enter your Pro license key in **Settings > License**.

## Settings reference

| Setting | Default | What it does |
|---|---|---|
| Baud rate | 115200 | Serial speed |
| Bed width / height | 220 / 220 mm | Drawing area limits |
| Pen up Z | 3 mm | Height while travelling |
| Pen down Z | 0 mm | Height while drawing (set in calibration) |
| Draw speed | 2000 mm/min | Speed with the pen down |
| Travel speed | 6000 mm/min | Speed with the pen up |
| Z speed | 500 mm/min | Speed of pen up/down moves |
| Path optimization | On | Reorders paths to reduce travel |
| Margin | 5 mm | Keeps drawings away from the bed edges |

## Project structure

```
creality-plotter-software/
  src/
    main/            Electron main process (serial, files)
    renderer/        UI (preview, controls, settings)
    core/
      image-to-svg/  Image tracing
      svg-to-gcode/  Path and G-code generation
      serial/        Printer connection and streaming
      handwriting/   Handwriting scan (Pro)
  assets/            Icons and images
  docs/              Extra docs and photos
  .github/workflows/ Build and release automation
  package.json
  README.md
  LICENSE
```

## Development

```bash
npm install     # install dependencies
npm start       # run the app in development
npm run lint    # check code style
npm test        # run tests
```

Main dependencies: Electron, Node.js, and [serialport](https://serialport.io/).

Keep secrets like license keys out of the code. Use a `.env` file (see `.env.example`). `.env` is listed in `.gitignore`.

## Building releases

```bash
npm run build:mac    # builds a .dmg
npm run build:win    # builds a Windows installer
```

Pushing a version tag such as `v1.0.0` triggers the GitHub Actions workflow, which builds for macOS and Windows and attaches the files to a GitHub Release.

## Troubleshooting

**The printer does not appear in the port list**
- Use a data-capable USB-C cable.
- Reconnect the cable with the printer powered on.
- On Windows, install the CH340 driver.
- Close other programs that may be using the port (slicers, terminal apps).

**Connected but nothing moves**
- Try baud rate 115200.
- Home the printer from its own screen first.
- Make sure the printer is not in the middle of a print job.

**Lines are faint or missing**
- Lower Pen Down Z a little and redo calibration.
- Check that the pen is not drying out.

**The pen tears the paper or drags**
- Raise Pen Down Z.
- Lower the draw speed.
- Make sure the paper is taped flat.

**The drawing is cut off or shifted**
- Check the bed size and margin settings.
- Make sure the printer was homed before plotting.

**The plot stops halfway**
- Check the USB cable and avoid unplugging it.
- Turn off computer sleep during long plots.

## FAQ

**Does this work with any 3D printer?**
It is designed for the Ender 3 V3 SE. Other printers that accept G-code over serial may work with manual settings.

**Does it work with other plotters?**
The image to SVG converter outputs standard SVG that other plotter software can use.

**Do I need the paid subscription?**
No. Plotting is free. Pro adds handwriting scanning.

**Can I use any pen?**
Most fineliners and gel pens work. Test with a cheap one first.

**Will this damage my printer?**
Used carefully, no. Always calibrate pen height first and keep the heaters off. See the disclaimer below.

## Roadmap

- [ ] USB-C serial connection (macOS and Windows)
- [ ] Automatic port detection
- [ ] Image to SVG
- [ ] SVG to G-code
- [ ] Live preview
- [ ] Start, pause, resume, stop
- [ ] Pen calibration helper
- [ ] Pro license check
- [ ] Handwriting scan
- [ ] Handwriting to font
- [ ] Profiles for other Creality printers
- [ ] Linux support

## Contributing

Contributions are welcome.

1. Fork the repo.
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit with a clear message.
4. Push and open a pull request.

Bug reports should include your OS, printer model, app version, and steps to reproduce.

## License

Released under the MIT License. See [LICENSE](LICENSE).

## Safety and disclaimer

- Keep the nozzle and bed heaters **off** while plotting.
- Never leave the printer running unattended.
- Make sure the pen holder is secure so it cannot fall into the mechanism.
- This project is not affiliated with or endorsed by Creality. Use at your own risk.
