# Open APS Explorer HAT EEPROM

The EEPROM settings are used with the Raspberry Pi HAT add-on board specification
to provide the product definition in the device tree and the initial GPIO settings
when the device boots.

## EEPROM Programming

Programming the EEPROM is only necessary when manufacturing or developing an
Explorer HAT. When acquiring a pre-manufactured Explorer HAT the EEPROM will be
delivered with the settings pre-programmed in the EEPROM.

For a new device the EEPROM is programmed through a specially prepared Raspberry
Pi Zero which is configured to program HAT EEPROMs.

Use the included eepromutils from the Raspberry PI HATs project to program the
EEPROM.

### Preparing Pi Zero

Follow the Raspbian installation instructions in the Explorer Hat software setup
instructions. This will provide the base operating system needed for the EEPROM
programming process.

The i2c-0 needs to be active for the EEPROM programming process. Edit /boot/config.txt
and add a line with the text "dtparam=i2c_vc=on" to enable use of the i2c-0 device
when the Raspberry Pi boots.

It may be necessary to update the Pi Zero's firmware.
> sudo apt-get install rpi-update
> sudo rpi-update

Copy the included eepromutils directory to the Pi Zero and follow the instructions
to build the tools and program the EEPROM.

1) Compile eeprom tools.
2) Create eeprom eep file from settings.
3) Create blank eep file.
4) Write blank eep file to EEPROM.
5) Write settings eep file to EEPROM.
