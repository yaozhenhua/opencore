This is the final EFI folder for booting macOS Big Sur on Lenovo ThinkPad X1 Carbon 2nd generation. The hardware
configuration of the machine is:

| Component | Spec                                      |
|-----------|-------------------------------------------|
| Processor | Intel i7-4600U vPro                       |
| Memory    | 8GB DDR3L-1600MHz                         |
| Storage   | M.2 SSD 256 GB                            |
| Display   | 14" 2560x1440 with multi-touch            |
| Camera    | HD720p                                    |
| Graphics  | Intel HD Graphics 4400                    |
| Audio     | Realtek ALC 3232                          |
| WLAN      | Intel wireless-AC 7260 with Bluetooth 4.0 |
| Other     | Ambient Light Sensor, fingerprint reader  |

Reference: https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_X1_Carbon_2nd_Gen.pdf

The computer is no longer available so this EFI is no longer under maintenance. Here is a list of notes I can recall:

- Most of SSDT files in ACPI folder are generated using SSDTTime, particular for resolving IRQ conflicts.
- Airportltlwm is used to support WiFi card, it worked well.
- To make Bluetooth work, IntelBluetoothFirmware, IntelBluetoothInjector, and IntelBTPatcher are added. Additionally,
  USB mapping must be done to specify which USB port is for internal Bluetooth.
- USB mapping is supported by USBToolBox and UTBDefault. In hindsight, UTBDefault should be replaced by proper mapping
  on Windows.
- AppleALC is to support the audio with proper `alcid` in config.plist.
- During installation, a random "device-id" is specified for iGPU otherwise UI doesn't show up. However, the parameter
  "AAPL,ig-platform-id" must be specified correctly.
- UEFI secure boot and Windows bit locker are not configured.

During the boot-up, for some reason the screen is blank initially. One must press and hold power button until the
display backlight is off, then press it again then everything will work. Sleep and wake work.
