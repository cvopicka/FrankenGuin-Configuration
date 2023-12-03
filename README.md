# FrankenGuin-Configuration

My Klipper Creality CR6-SE Setup

## System Installation

System installation was actually fairly simple.

- [Install the Raspberry Pi Imager utility](https://www.raspberrypi.com/software/)
  - From in the imager I navigated to a lite distrobution as I was not needing a desktop OS.  Ultimately I settled on ***Raspberry Pi OS Lite (64 bit)***
  - The next step is to install KIAUH and this specifically says to use the 32-bit version.  I do not know why that is recomended as I have encountered no issues thus far.  The 64-bit version is "new" and likely not considered fully baked.  The 32-bit version likely would be just fine.
- [Install KIAUH](https://github.com/dw-0/kiauh)

    ```bash
    ssh me@frankenguin
    sudo apt-get update
    sudo apt-get install git
    git clone https://github.com/dw-0/kiauh.git
    ./kiauh/kiauh.sh
    ```

- Install Klipper
- Install Moonraker
- Install Fluidd
- Install KlipperScreen
- Install Mobileraker
- Don't install Crowsnest at this time

This is wehre I took a hybrid approach and copied from the Klipper Config Examples

```bash
cp ~/klipper/config/printer-creality-cr6se-2021.cfg ~/printer_data/config/printer.cfg
```

AND the CR6.cfg from ***KoenVanduffel's CR-6 Klipper repo*** (see links section)

I had to remember to add an include for this cfg file that I placed in the ~/printer_data/config/ folder.

This is where I later was struck from a problem with Fluidd.  Ultimately the solution was to add to the top of my printer.cfg

```text
[include CR6.cfg]
[include fluidd.cfg]
```

REBOOT

## Creating firmware.bin

```bash
cd klipper
sudo service klipper stop
make menuconfig
```

- select microcontroller - "STM32"
- processor model "STM32F103" (is the default)
- Bootloader - "28KiB"
- Communication interface "Serial (on USART1 PA10/PA9)"
- Quit (Q) and save the make command

```bash
make
```

Copy the firmware to an SD card formatted as FAT32 w/ 4096 bit sector size.  ALL machines require this to flash as far as I can tell from the several brands I have been exposed to.

```bash
cp ~/klipper/out/klipper.bin /SOME_MOUNTED_SD/firmware[yyyymmddhhss].bin
```

The CR6 will not install a firmware named the same as the last firmware.  I augment the data code to it so it is always unique.  This will help if you have to flash multiple times.  I did not.

Insert the SD card and start the printer.  Wait a while.  I don't remember any indication.

Connect the USB to the printer and restart the printer.

Edit the printer.cfg [mcu] section and update serial: to reflect your connection to the mcu.  For mine:

```text
serial: /dev/serial/by-id/usb-1a86_USB_Serial-if00-port0
```

REBOOT EVERYTHING.  Why not... I suggest full power cycle with a minimum of 30 seconds off.

## Calibration

[Stuff](https://www.klipper3d.org/Config_checks.html)

## Links

This was heavily informed by [KoenVanduffel's CR-6 Klipper repo](https://github.com/KoenVanduffel/CR-6_Klipper)
