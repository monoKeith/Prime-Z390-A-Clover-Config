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
* DRAM Frequency: DDR4-2666Mhz [I have stability issues (display halt) if I run at memory's native 3200Mhz]
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

## Boot

### Boot Configuration
* Fast Boot: Disabled

### Secure Boot
* OS Type: Windows UEFI Mode

---
