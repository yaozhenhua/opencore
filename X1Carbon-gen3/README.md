This is the EFI folder using OpenCore 0.9.4 to boot up macOS Monterey on Lenovo ThinkPad X1 Carbon 3rd generation. The
hardware specification is the following:

| Component  | Spec                                  |
|------------|---------------------------------------|
| Processor  | Intel i7-5600U vPro                   |
| Graphics   | Intel HD 5500                         |
| Display    | 14" 2560x1440 with multi-touch        |
| Memory     | 8 GB DDR3L PC3-12800                  |
| Storage    | M.2 SSD 512 GB                        |
| WLAN       | Intel Wireless-AC 7265 with Bluetooth |
| Audio      | Realtek AL3232                        |
| Camera     | HD720p                                |
| Other      | fingerprint reader                    |

Reference: https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_X1_Carbon_3rd_Gen.pdf

Notes:

- The boot loader `Boot/bootx64.efi` is part of Windows Boot Manager unmodified, not from OpenCore. If something
  breaks, Windows can still boot. The computer is set to boot "Windows Boot Manager" which passes control to OpenCore.
  This is done by "bcdedit /set {bootmgr} path \EFI\OC\OpenCore.efi" command on Windows.

- Some files in ACPI folder are pre-generated and downloaded. Some are created using SSDTTime, particularly for
  resolving IRQ conflicts. SSDT-SBUS-MCHC is added to deal with SMBus support per instruction at [Dortania
  guide](https://dortania.github.io/Getting-Started-With-ACPI/Universal/smbus.html)

- About Kext files:
  - Airportltlwm is working well and performance is okay, location support and native WiFi are working.
  - Bluetooth support is handled by BlueToolFixup, IntelBluetoothFirmware, IntelBTPatcher.
  - ECEnabler is for the battery status.
  - IntelMausi is the Ethernet driver, in reality I am not using it.
  - NVMeFix is to handle the M.2 SSD which isn't Apple.
  - USB mapping is done properly by USBToolBox and UTBMap, and generated on Windows before installation.
  - VoodooPS2Controller alone is sufficient to support keyboard, trackpoint, and trackpad.

- Command and option keys are swapped by changing parameter in plist file in VoodooPS2keyboard.

- UEFI secure boot has not been enabled yet (don't want to enroll the keys since it broke P52s). Windows bitlocker is
  not enabled either.

- During installation, "device-id" in iGPU can be set to a random number to see the screen properly (although without
  acceleration). After installation, this should be fixed to the proper value, and then the display will be blank after
  boot, then press and hold power button to turn off the display backlight, press any key to bring it back to life.

- Not sure why, after sleep and wake the power LED will keep flashing. But everything else works.

- CustomSMBIOSGuid and UpdateSMBIOSMode are changed to not apply the SMBIOS change to Windows. Otherwise, WLAN device
  will fail to start with error code 10 (bad data).
