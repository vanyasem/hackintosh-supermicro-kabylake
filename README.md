# Supermicro MBD-X11SAE-O Hackintosh

**Supported macOS version:** macOS 12 - Monterey

**SMBIOS:** iMac18,3 _(use iMac18,1 for builds without dGPU)_

**OpenCore:** [0.7.7](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.7.7)

## Hardware

- **Motherboard**: Supermicro MBD-X11SAE-O
- **GPU**: Gigabyte PCI-E Radeon RX 570 8GB _(GV-RX570GAMING-8GD-MI)_ - **Polaris**
- **CPU**: Intel Xeon E3-1225 v6 _(cm8067702871024s r32c)_ - **Kaby Lake**

## Quirks

Only one of the 2 Ethernet ports work in macOS. The left Ethernet port (closer to display cables) works.

However, if you plug anything into the right Ethernet port, the system will kernel panic soon after boot. There are multiple reports of the issue for different SuperMicro motherboards, and I was not able to find a fix for the issue.

## Notes

While the config itself is ready to use, you still need to [generate your own serial numbers and tweak the config accordingly](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#using-gensmbios) before installing the system.

SuperMicro does not allow to change CFG lock in UEFI settings, so you will have to manually patch your UEFI to remove CFG lock. You will have to follow [this guide](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#checking-if-your-firmware-supports-cfg-lock-unlocking) first before attempting to install macOS. It's perfectly safe to do, as long as you understand the instructions given, and perform them exactly as described.

After installing, you might be interested in [enabling secure boot](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html#dmgloading). I have only tested the "Default" option.

You can also [disable on-screen logging](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/debug.html#config-changes) once you're sure that your system is stable.

Boot chime is enabled to be played on the back orange AUX port.

Alternative (quiter) boot chime provided in [Resources/Audio](Resources/Audio)

`ACPI` patches are included, and are specific to the motherboard. They were made by me on 11-Aug-2021. If you have a different motherboard, you will need to follow [this guide](https://dortania.github.io/Getting-Started-With-ACPI/) to create your own suitable for your motherboard.

`USBMap.kext` is specific to the motherboard, and was manually [generated](https://github.com/corpnewt/USBMap) by me on 11-Aug-2021

If you have a different motherboard, you have to generate your own `USBMap` yourself. Read [https://dortania.github.io/OpenCore-Post-Install/usb/](this) for more details.

If you choose to try this config on a different motherboard anyways, you will also probably need to adjust the `layout-id` to get your audio ports detected correctly. Follow [this guide](https://dortania.github.io/OpenCore-Post-Install/universal/audio.html#finding-your-layout-id).

## Bundled

[Resources](https://github.com/acidanthera/OcBinaryData) included

### Drivers

1) `AudioDxe.efi` - [HDA audio support driver](https://dortania.github.io/docs/latest/Configuration.html#audiodxe) EFI firmware for most Intel and some other analog audio controllers
2) `HfsPlus.efi` - Proprietary EFI **HFS+** file system driver
3) `OpenCanopy.efi` - OpenCore plugin (Bootloader GUI)
4) `OpenRuntime.efi` - Mandatory OpenCore plugin with [special features](https://dortania.github.io/docs/latest/Configuration.html#openruntime)

### Kexts

1) [AppleALC](https://github.com/acidanthera/AppleALC) (**1.6.8**) - Audio for not officially supported codecs
2) [IntelMausi](https://github.com/acidanthera/IntelMausi) (**1.0.7**) - Intel Ethernet LAN driver
3) [Lilu](https://github.com/acidanthera/Lilu) (**1.5.9**) - Required for almost all other Kexts
4) [NVMeFix](https://github.com/acidanthera/NVMeFix) (**1.1.1**) - Improve compatibility with non-Apple NVMe SSDs
5) [SMCProcessor](https://github.com/acidanthera/VirtualSMC) (**n/a**) - Part of VirtualSMC
6) [SMCSuperIO](https://github.com/acidanthera/VirtualSMC) (**n/a**) - Part of VirtualSMC
7) USBMap - *Generated manually as part of SSDT patches, preferred*
8) [VirtualSMC](https://github.com/acidanthera/VirtualSMC) (**1.3.2**) - SMC emulator layer
9) [WhateverGreen](https://github.com/acidanthera/WhateverGreen) (**1.6.6**) - Patches for GPUs

### Tools

1) `OpenShell.efi` - EFI shell, useful for debugging
