# ASUS Prime Z390-A Clover Bootloader Configuration

## Specification
* CPU: Intel Core i9 9900K
* Motherboard: ASUS Prime Z390-A (BIOS Release Date: 2020/05/04 Version: 1502)
* Memory: 2 * Corsair Vengeance LPX 16GB 3200Mhz
* GPU: MSI Radeon RX 5700 XT Gaming X 
* SSD: Western Digital WD Black SN750 500GB
* Wireless: Fenvi T919

---
## BIOS Settings (For Reference ONLY)

### Ai Tweaker
* Ai Overclock Tuner: XMP II
* ASUS MultiCore Enhancement: Enabled - Remove All limits
* DRAM Frequency: DDR4-2933Mhz 

    I have stability issues (display halt) if I run at memory's default 3200Mhz
    
    This is probably due to BCLK/DRAM multiplier, the real iMac 19,1 runs its memory at  2666Mhz , which means multiplier was set to 100:133. I found [a comment](https://www.overclock.net/forum/18051-memory/1704874-133-mode-vs-100-mode.html) saying "some platforms do not like high memory multipliers", but I need to test it out.

* DRAM Voltage: 1.30000 [Unnecessary RAM tweeking]


### Advanced
#### Platform Misc Configuration
* PCI Express Native Power Management: Enabled
* Native ASPM: Enabled

#### CPU
* Software Guard Extensions (SGX): Disabled
* Intel (VMX) Virtualization Technology: Enabled
    #### CPU - Power Management Control
    * Boot performance mode: Turbo Performance
    * Turbo Mode: Enabled
    * CFG Lock: Disabled

#### System Agent (SA) Configuration
* VT-d: Disabled
* Above 4G Decoding: Enabled
    #### Graphics Configuration
    * Primary Display: PCIE
    * iGPU Multi-Monitor: Enabled
    * DVMT Pre-Allocated: 64M

#### PCH Configuration
PCI Express Configuration -> PCIe Speed: Gen3
[This probably doesn't do anything]

#### PCH Storage Configuration
* SATA Controller(s): Disabled [I don't use any SATA Drives]

#### Onboard Devices Configuration
* PCIEX16_3 Bandwidth: X4 Mode [I have my SSD installed on that port]
    #### Serial Port Configuration
    * Serial Port: OFF

#### USB Configuration
* Legacy USB Support: Disabled
* XHCI Hand-off: Enabled

### Boot

#### Boot Configuration
* Fast Boot: Disabled

#### Secure Boot
* OS Type: Windows UEFI Mode

---
## IO Ports

By using [Hackintool](https://github.com/headkaze/Hackintool), I selected 15 USB ports that I needed and blocked the rest of them. You probably need different configuration to make your setup works (either reconfigurate or avoid using ports that are blocked).

* Green: Ports that works
* Red: Ports that won't work 

![Rear](/Pictures/rearIO.png)

* Some devices that are not being supported on macOS will cause problems. For example, my Oculus Rift S (which doesn't have software support under macOS) causes instant wakeup, the solution is to plug it into one of the ports that are blocked, so it won't even show up under IORegistryExplorer.

* The configuration was set to use discrete graphics, so the onboards video output won't work.

![Front](/Pictures/frontIO.png)

* I used the bottom connector from USB1112 for Bluetooth connection integraded to my wireless card (Fenvi T919). USB_E12 and USB_E34 was connected internally via a USB 2.0 Hub, not only makes the USB device tree looks messy, but also brings some chances of not being able to discover Bluetooth hardware if the Bluetooth USB is connected to one of them.

* Although I blocked USB_E12 and USB_E34 in the configuration, they are still useful for connecting devices that works on Windows but not needed for macOS, like AIO water pump and RGB controller.

* My case doesn't have a front USB-C connector, so the U31G1_C5 is blocked.

---
## Before booting up

* I removed HFSPlus driver because I don't need it after installation. If you need to boot installer from USB, just add HFSPlus.efi to CLOVER/drivers/UEFI

## Before signing in Apple ID

Open Clover config file with Clover Configurator

![SMBIOS](/Pictures/SMBIOS.png)

* Click 'Generate New' for Serial Number and SmUUID, then click 'check coverage' to make sure the serial number isn't occupied by a real Mac.

![systemPref](/Pictures/systemPref.png)

* Click 'Generate New' Custom UUID.

---
## Arguments in config file

* slide=2048

    This vlaue is ONLY useful if kernel panic happens with memory allocation issues.
    
    It was calculated according to [Fixing KASLR slide values](https://dortania.github.io/OpenCore-Desktop-Guide/extras/kaslr-fix.html).

* agdpmod=pikera

    [WhateverGreen](https://github.com/acidanthera/WhateverGreen) argument for fixing RX 5700 XT black screen issue.


---
## SSDT Patches

* SSDT-SET-STAS.aml

    AWAC patch for Z390 series. Downloaded from [here](https://www.binss.me/blog/solve-hackintosh-stuck-problem-after-bios-upgrading/).

* SSDT-PM.aml

    NVRAM support on Z390 series. Downloaded from [here](https://www.insanelymac.com/forum/topic/342183-native-nvram-for-z390-chipset-ssdt/).

* SSDT-EC-USBX.aml

    USB injection patch. Generated by [Hackintool](https://github.com/headkaze/Hackintool).

---
## Kexts

* AirportBrcmFixup.kext

    This kext is needed for better wireless support for my FV-T919 wireless card. Although the card works witout this kext, it gets painfully slow and the connection crashes frequently.

* DAGPM.kext

    This kext is needed for enabling macOS native power management for GPU, by injecting necessary information. This kext was originally found [here](https://www.tonymacx86.com/threads/macos-native-discrete-gpu-power-management.247479/), I followed the guide and modified it to fit the SMBIOS of iMac19,1 it won't work on any other SMBIOS without modifications.

    You can verify if it working by checking this directory:
    <cite>IOService:/AppleACPIPlatformExpert/PCI0@0/AppleACPIPCI/PEG0@1/IOPP/GFX0@0/IOPP/pci-bridge@0/IOPP/display@0/AMDRadeonX6000_AmdRadeonControllerNavi10/ATY,Adder@0/AGPM</cite>

---
## What Works?

Native CPU power management / iGPU frequency management

![CPU](/Pictures/CPU.png)

Intel iGPU H.264/ HEVC Acceleration

![iGPU](/Pictures/iGPU.png)

Radeon RX 5700 XT pretended to be a Radeon Pro W5700X

![dGPU](/Pictures/dGPU.png)

---
## Benchmark

Geekbench 5:

![CPU_Bench](/Pictures/CPU_Bench.png)

![Metal_Bench](/Pictures/Metal_Bench.png)

![OpenCL_Bench](/Pictures/OpenCL_Bench.png)
