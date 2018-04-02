# Ryzen Hack-Pro
- Asrock AB350M Pro4
- Ryzen 1700 _(stock)_
- 16GB _(3200 MHz)_
- Asus GTX 960 2GB _[(GTX960-DC2OC-4GD5-BLACK)](https://www.asus.com/Graphics-Cards/GTX960-DC2OC-4GD5-BLACK/)_
- 500GB Samsung 960 EVO _(NVMe)_

# Create USB
~Format drive as Mac OS Extended (Journaled)~ _just use transmac_

Download HighSierraAMD_V2 UEFI [HighSierraAMD V2](https://goo.gl/czVVY8)

Restore to usb using [transmac](https://www.acutesystems.com/scrtm.htm) _(Disk Utility wont create EFI)_

# Mount USB as EFI and add OsxAptioFixDrv-64.efi
```bash
diskutil list
diskutil mount disk1s2
# replace disk1s2 with correct usb disk/partition
```

Download [OsxAptioFixDrv-64.efi](https://drive.google.com/file/d/1yjVVZddnvvfYcf5ha9JCH5LOw1-894RP/view?usp=sharin) and copy to `EFI/CLOVER/drivers64UEFI`

```bash
cp ~/Downloads/OsxAptioFixDrv-64.efi /Volumes/EFI/EFI/CLOVER/drivers64UEFI/
```

# BIOS
Configure the following BIOS options
- disable serial port
- set SATA to AHCI
- legacy secure boot

# Preinstall
__Plug usb drive into usb 2.0 port__

Boot into usb Clover using UEFI entry

Set Clover options: `-v set npci=0x2000 nv_disable=1`

Boot __macOS from HighSierraAMD__ _(usb drive is named __HighSierraAMD__)_

Use __Disk Utility__ to format drive to MacOS Extended (Journaled) _NVMe drive may be external, this is ok._
Remember drive name _Ex.`960EVO`_

Then return to the main screen and __Reinstall MacOS to new drive__

Reboot back into usb Clover, use same options as above

Open __Terminal__ and type `preinstall`, then type drive name from above (`960EVO`)


# Install

Reboot back into usb Clover, then boot to __macOS install from 960EVO__

# Post Install

Reboot back into usb Clover, then boot to __macOS from HighSierraAMD__

Open __Terminal__ and type

- `nvweb` to install Nvidia Web Drivers
- `amd`, then type drive name from above (`960EVO`) to install AMD patched kernel


# Clover Configurator

[Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/)

Copy the existing nvidia config.plist on the USB and make the following edits

- __Install Drivers:__ `OsxAptioFix3Drv-64` _(remove any other OsxAptioFix)_

- __Boot:__ In _Custom Flags_ remove `-radoff`

- __SMBIOS:__ Set to `MacPro6,1`

- __System Parameters:__ Make sure `NvidiaWeb` is checked


# LINKS
nvme internal https://forum.amd-osx.com/viewtopic.php?f=52&t=4154


