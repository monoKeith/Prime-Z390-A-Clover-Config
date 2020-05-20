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
* DRAM Frequency: DDR4-2666Mhz [I have stability issues (display halt) if I run at memory's default 3200Mhz]
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
## Setting up the Radeon RX 5700 XT

Boot into Windows, check graphics card revision and board ID using [GPU-Z](https://www.techpowerup.com/gpuz/).

![GPU-Z](/Pictures/graphicsRevision.PNG)

* Just hover the mouse on BIOS Version

These two numbers correspond to ROM revision and EFI Driver version on macOS, we need to put the correct number in Clover configuration.

![Graphics](/Pictures/graphics.png)

Open Clover config file, update the corresponding value to match your Radeon RX 5700 XT.

![Revision](/Pictures/gpuRevision.png)

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
## Benchmark

Geekbench 5:

![CPU](/Pictures/CPU.png)

![OpenCL](/Pictures/OpenCL.png)

![Metal](/Pictures/Metal.png)