# ryzen hack pro
- __Asrock AB350M Pro4__ _4.70 BIOS_
- __Ryzen 1700__ _stock_
- __16GB__ _3200 MHz_
- __Asus GTX 960 2GB__ _[GTX960-DC2OC-4GD5-BLACK](https://www.asus.com/Graphics-Cards/GTX960-DC2OC-4GD5-BLACK/)_
- __500GB Samsung 960 EVO__ _NVMe_
- 10.13.3 High Sierra

## What doesn't work
- NVMe drive is seen as "external"
- GPU performance is about half of what it should be

# Install steps
## create USB
Download HighSierraAMD_V2 UEFI [HighSierraAMD V2](https://goo.gl/czVVY8)

Restore to USB using [transmac](https://www.acutesystems.com/scrtm.htm) _(Disk Utility wont create EFI)_

Mount USB EFI and copy [OsxAptioFixDrv-64.efi](https://drive.google.com/file/d/1yjVVZddnvvfYcf5ha9JCH5LOw1-894RP/view?usp=sharin) to `EFI/CLOVER/drivers64UEFI`

### Notes
Might just be easier to install __OsxAptioFix3Drv-64.efi__ to the USB EFI using Clover Configurator

Add __apfs.efi__ too if missing


## BIOS
Configure the following BIOS options
- disable serial port
- set SATA to AHCI
- legacy secure boot (enable CSM, but set to Legacy)

## install
__Plug USB drive into USB 2.0 port__

Boot into USB Clover using UEFI entry

Set Clover options: `-v set npci=0x2000 nv_disable=1` (use these options for the rest of the install too)

Select __Boot macOS from HighSierraAMD__ _(USB drive is named __HighSierraAMD__)_

Use __Disk Utility__ to format drive to MacOS Extended (Journaled) and name it `SSD`
- NVMe drive may be external, this is ok.
- drive name can be anything, adjust steps below accordingly.

Return to the main screen and select __Reinstall MacOS to new drive__

Reboot into USB Clover, select __Boot macOS from HighSierraAMD__

Open __Terminal__ and run `preinstall`, enter drive name `SSD` when prompted

Reboot into USB Clover, __Boot macOS install from SSD__

Reboot into USB Clover, __Boot macOS from HighSierraAMD__

Open __Terminal__ and type
- `nvweb` to install Nvidia Web Drivers
- `amd`, then type drive name `SSD` to install AMD patched kernel


## Clover Configurator

Download [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/) and install to `SSD`.

Because the latest Clover (4411) doesn't work with Ryzen, use the USB installer's Clover

Use __Clover Configurator__ to mount both SSD and USB EFI and overwrite the entire SSD EFI with the USB EFI

Make sure the config.plist has the following edits

- __Install Drivers:__ `OsxAptioFix3Drv-64` _(remove any other OsxAptioFix)_
- __Boot:__ In _Custom Flags_ remove `-radoff`
- __SMBIOS:__ Set to `MacPro6,1`
- __System Parameters:__ Make sure `NvidiaWeb` is checked


# 10.13.3 Update

The 378.10 Nvidia driver does not support HDMI, however the 387.10 driver _does_, but it requires >=10.13.3.

According to the forums the AMD kernel patch currently has issues with 10.13.4, so we'll use 10.13.3 instead.

## update steps

Download the [10.13.3](https://support.apple.com/kb/DL1954) update and start the install.

Reboot into USB Clover, __Boot macOS from HighSierraAMD__, run `preinstall`, enter drive name `SSD`

Reboot into USB Clover, __Boot macOS install from SSD__, let it complete install

Reboot into USB Clover, __Boot macOS from HighSierraAMD__, run `amd`, enter drive name `SSD`

Reboot into SSD Clover, __Boot macOS install from SSD__, install the correct nvidia web driver for your 10.13.3 build and reboot.
