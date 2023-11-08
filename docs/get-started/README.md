<!-- docs/get-started/README.md -->

# Get Started!

---

## Usage (rev 0.1d/e)

### Power supply

The modem can be supplied with:
- a 6..15V source via the DC plug (upper right hand side) or pin 9 of the DE9 connector (upper left hand side),
- 5V through the USB-C connector (in the middle of the upper side)
Both inputs can be used at the same time.

### DE-9 connector

The DE-9 connector at the top of the board can be used to connect Module17 to a 9600baud radio. The pinout is shown in the table below.

| Pin      |             Function             |             Direction            | `CT-167` cable wire color |
|----------|:--------------------------------:|----------------------------------|---------------------------|
| 1        |  unused (floating)               |  --                              |                           |
| 2        |  baseband output (towards radio) |  output                          |  brown                    |
| 3        |  CAT-RX                          |  input                           |                           |
| 4        |  CAT-TX                          |  output                          |                           |
| 5        |  radio PTT                       |  output, open-drain, low-active  |  red                      |
| 6        |  baseband input (from radio)     |  input                           |  orange                   |
| 7        |  unused (floating)               |  --                              |                           |
| 8        |  ground                          |  --                              |  black, thick             |
| 9        |  12V input                       | input (supply)                   |                           |

![CT-167 Wiring](_media/CT-167.png ':size=75%')

Additionally, there is a 2.54mm pin header just next to the DE-9 connector that can be used for convenient access to the baseband, PTT and CAT signals. Pin 9 doesn't have to be used, it only provides an alternative way for supplying the board.  

### Kenwood mic-speaker connector

On the left hand side of the board, there is a Kenwood-type connector. The pinout is standard and most microphone-speakers should be compatible with Module17.

### Volume knob with a power switch

The volume knob is at the bottom of the board. As the name suggests, it is used for audio volume setting. It also acts as the power switch for the module. **Note** - the power switch is on the 12V line, so it is not possible to turn the device off while it is powered with 5V (USB).

### Transmission/reception

At idle, the device will look for valid M17 signal in the baseband. If there is a valid signal detected carrying voice data, it will be decoded and sent to the speaker at the Kenwood  connector. There is also an additional, unpopulated, 2-pin speaker connector in the lower left corner of the modem. It can be used to connect an external >=8ohms speaker.
Transmission is triggered by the PTT key of the mic-speaker. A valid baseband along with a PTT signal is then sent to the radio.

**Warning** - OpenRTX does not yet support phase inversion of the baseband. Channel Access Number is not settable. CAT protocol is not implemented yet. (November 2022)

---

## Flashing

### Linux

You need `dfu-util` 0.11 to start the process. `dfu-util` installation instructions are [here](https://dfu-util.sourceforge.net/build.html). It is recommended to use the most recent version - built from source.

To flash, hold the "Escape" (upper left) button down while plugging in USB (or pressing the reset switch) to boot into DFU mode.

For your convenience, a pre-built binary is available [here](https://files.openrtx.org/nightly/) and [here](https://openrtx.schinken-radio.de/nightly/). The name of the file is `openrtx_mod17_wrap`.
You can then follow standard [OpenRTX](https://github.com/OpenRTX/OpenRTX)
flashing instructions for the "mod17" target if you have dfu-util
installed:<br>
`dfu-util -d 0483:df11 -a 0 -D openrtx_mod17_wrap -s 0x08000000`

Once flashing is complete, reset the board to boot into the newly flashed application. If there are any errors while flashing, make sure there's a valid udev rule available for the Module17. Download the [udev](https://github.com/OpenRTX/OpenRTX/blob/master/99-openrtx.rules) file and then run the following commands:<br>
`sudo cp 99-openrtx.rules /etc/udev/rules.d`<br>
`sudo udevadm control --reload-rules`.

### Windows
Be sure that you have WinUSB installed for your DFU device. You can use [Zadig](https://zadig.akeo.ie/). To enter the DFU mode, hold down the upper left button (below the display) while plugging the USB-C cable in. The button can now be released. Nothing should appear on the display at this moment. After the Module17 is connected and in DFU mode, select it from the list in Zadig, then install the driver.

- Download the [dfu-util-0.11-binaries.tar.xz dfu-util binary for Windows](https://dfu-util.sourceforge.net/releases/) and unzip.
- Download the `openrtx_mod17_wrap` file, it's available [here](https://files.openrtx.org/nightly/) and [here](https://openrtx.schinken-radio.de/nightly/).
- Navigate to `{extraction_location}\dfu-util-0.11-binaries\win64` (or `win32`) and copy the wrap file there. Example path: `C:\Users\SP5WWP\Desktop\dfu-util-0.11-binaries\win64`.
- Run the command prompt (`cmd`) as administrator and change the working directory with `cd {extraction_location}\dfu-util-0.11-binaries\win64`.
- Run the following command: `dfu-util -d 0483:df11 -a 0 -D openrtx_mod17_wrap -s 0x08000000`
- After the flashing is complete, close the command prompt and reset the device.

### Building the firmware yourself
Building instructions are available [at the OpenRTX project's website](https://openrtx.org/#/compiling).<br>
**Note**: it is not yet possible to change the station's callsign using the GUI. To change it, please edit [line #47 of the state.c file](https://github.com/OpenRTX/OpenRTX/blob/master/openrtx/src/core/state.c#L47) before compiling.