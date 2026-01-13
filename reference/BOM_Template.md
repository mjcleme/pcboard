# Bill of Materials (BOM) Template

## Overview
This document provides a template and guidance for the complete Bill of Materials for the Intel i9 LGA1700 motherboard design.

## BOM Organization

### Category Breakdown
1. Processor/Socket
2. Memory
3. Power Delivery (VRM)
4. I/O Connectors
5. Clock Generation
6. Storage Interfaces
7. Audio
8. Network
9. Chipset
10. Passive Components
11. Mechanical

## Critical Components

### 1. CPU Socket

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U1 | LGA1700 Socket | LGA1700-SOCKET | Foxconn/Lotes | 1 | $15-25 | 1700-pin socket for Intel 12/13/14 gen |

### 2. Memory Connectors

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| J1-J4 | DDR5 DIMM Socket 288-pin | DDR5-288P-SOCKET | TE Connectivity | 4 | $2-4 | Standard DDR5 DIMM socket |

### 3. VRM Components

#### VRM Controller
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U2 | 18-Phase PWM Controller | RAA229618 | Renesas | 1 | $15-30 | VR13.5 compliant, SVID interface |

#### Power Stages (VCC Rail - 16 phases)
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U10-U25 | 90A Power Stage (DrMOS) | TDA21490 | Infineon | 16 | $2.50-4.00 | Integrated driver + MOSFETs |

#### Power Stages (VCCIN - 4 phases)
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U26-U29 | 90A Power Stage (DrMOS) | TDA21490 | Infineon | 4 | $2.50-4.00 | Same as VCC rail |

#### Power Stages (Other Rails)
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U30-U35 | 60A Power Stage (DrMOS) | SIC632 | Vishay | 6 | $1.50-3.00 | For VCCSA, VDD, VCC_GT (2 phases each) |

#### VRM Inductors
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| L1-L16 | Inductor 0.47µH 50A | IHLP-5050CE-01 | Vishay | 16 | $1.00-2.00 | For VCC phases |
| L17-L20 | Inductor 1.0µH 50A | IHLP-5050CE-01 | Vishay | 4 | $1.00-2.00 | For VCCIN phases |
| L21-L26 | Inductor 0.68µH 40A | IHLP-4040DZ | Vishay | 6 | $0.80-1.50 | For other rails |

#### VRM Capacitors - Input (12V)
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| C1-C10 | Polymer Cap 470µF 25V | 25ZLH470M | Rubycon | 10 | $0.50-1.50 | Low ESR bulk input |

#### VRM Capacitors - Output (VCC Rail)
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| C100-C119 | Polymer Cap 470µF 6.3V | POSCAP TPE | Panasonic | 20 | $0.30-0.80 | Bulk output |
| C120-C139 | Polymer Cap 220µF 6.3V | POSCAP TPD | Panasonic | 20 | $0.25-0.60 | Medium bulk |
| C140-C159 | Polymer Cap 100µF 6.3V | POSCAP TPC | Panasonic | 20 | $0.20-0.50 | Response |
| C160-C199 | Ceramic 10µF X7R 10V 1206 | GRM31CR71A106KA01L | Murata | 40 | $0.05-0.15 | High frequency |
| C200-C259 | Ceramic 1µF X7R 10V 0805 | GRM21BR71A105KA01L | Murata | 60 | $0.02-0.08 | Very high freq |
| C260-C339 | Ceramic 0.1µF X7R 16V 0603 | GRM188R71C104KA01D | Murata | 80 | $0.01-0.03 | Ultra high freq |

#### VRM Capacitors - Other Rails
| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| C400-C449 | Mixed polymer/ceramic | Various | Various | 50 | $0.05-0.50 | VCCIN, VCCSA, VDD, VCC_GT |

### 4. Power Connectors

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| J10 | ATX 24-pin Power Connector | 0475710001 | Molex | 1 | $0.50-1.50 | Main power input |
| J11 | EPS 8-pin 12V Connector | 0455960001 | Molex | 1 | $0.30-1.00 | CPU power (can use 2× for high-end) |

### 5. USB-C Interface

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| J20 | USB Type-C Receptacle | DX07S024JJ1R1500 | JAE | 1 | $0.50-2.00 | 24-pin mid-mount |
| U40 | USB PD Controller | TPS65987D | Texas Instruments | 1 | $3.00-6.00 | Optional, for PD support |
| U41 | USB Load Switch | TPS2561 | Texas Instruments | 1 | $0.20-0.80 | VBUS switching |
| U42 | ESD Protection IC | TPD8S009 | Texas Instruments | 1 | $0.30-1.00 | Multi-channel TVS |

### 6. PCH (Platform Controller Hub)

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U50 | PCH Chipset | Z790/Z690 | Intel | 1 | $40-80 | Requires NDA/authorization |

**Note**: Intel chipsets require authorization and are typically only available to authorized manufacturers. Alternative: Use reference design or work with Intel.

### 7. Clock Generation

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U60 | Clock Generator | 9DBL0951AKLF | Renesas | 1 | $2.00-5.00 | Generates system clocks |
| Y1 | Crystal 25MHz | Various | Various | 1 | $0.50-2.00 | Reference crystal |

### 8. BIOS/UEFI Flash

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U70 | SPI Flash 128Mbit | W25Q128JV | Winbond | 1 | $0.50-1.50 | BIOS storage |

### 9. Additional I/O Connectors

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| J30-J33 | USB 3.0 Type-A Receptacle | Various | Various | 4 | $0.30-1.00 | Rear I/O USB ports |
| J40-J43 | USB 2.0 Header | Various | Various | 4 | $0.10-0.30 | Internal headers |
| J50 | Audio Jacks (3.5mm) | Various | Various | 3-6 | $0.20-0.50 | Line out, mic, etc. |
| J60 | RJ45 Ethernet Jack | Various | Various | 1 | $0.50-2.00 | Gigabit/2.5G/10G |
| J70-J73 | SATA Connectors | Various | Various | 4 | $0.15-0.40 | For HDDs/SSDs |
| J80-J81 | M.2 Socket | Various | Various | 2 | $0.50-1.50 | M.2 SSD slots |

### 10. PCIe Slots

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| J100 | PCIe x16 Slot (Gen 5) | Various | Various | 1-2 | $2.00-5.00 | Primary GPU slot |
| J101-J103 | PCIe x1 Slot (Gen 4) | Various | Various | 3 | $0.50-1.50 | Expansion cards |

### 11. Audio Codec

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U80 | Audio Codec | ALC4080/ALC1220 | Realtek | 1 | $2.00-5.00 | High-end audio |

### 12. Network Controller

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U90 | Gigabit Ethernet PHY | RTL8111H | Realtek | 1 | $1.00-3.00 | Or 2.5G/10G option |

### 13. Voltage Regulators (Low Power Rails)

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| U110-U115 | 3.3V/5V/1.8V LDO | Various | Various | 6 | $0.20-1.00 | Auxiliary rails |

### 14. Passive Components Summary

#### Resistors
| Description | Package | Qty | Unit Price | Notes |
|-------------|---------|-----|------------|-------|
| 0603 resistors (various values) | 0603 | 400 | $0.001-0.01 | Typical values: 10Ω-100kΩ |
| 0805 resistors (various values) | 0805 | 100 | $0.002-0.02 | Higher power |
| Pull-up/down for USB-C | 0603 | 10 | $0.001-0.01 | 5.1kΩ, 10kΩ, 22kΩ, 56kΩ |

#### Capacitors (Beyond VRM)
| Description | Package | Qty | Unit Price | Notes |
|-------------|---------|-----|------------|-------|
| 10µF X7R 10V | 0805/1206 | 150 | $0.05-0.15 | General decoupling |
| 1µF X7R 10V | 0603/0805 | 200 | $0.02-0.08 | General decoupling |
| 0.1µF X7R 16V | 0402/0603 | 300 | $0.01-0.03 | General decoupling |
| 22µF X7R 6.3V | 1206 | 50 | $0.10-0.30 | Memory power |

#### Ferrite Beads
| Description | Package | Qty | Unit Price | Notes |
|-------------|---------|-----|------------|-------|
| Ferrite beads (various) | 0603/0805 | 30 | $0.02-0.10 | EMI filtering |

### 15. Mechanical Components

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| - | Mounting holes | - | - | 6-9 | - | ATX standard pattern |
| - | Standoffs (if needed) | Various | Various | 10 | $0.05-0.20 | Board support |
| - | VRM Heatsink | Custom/Aftermarket | Various | 1 | $10-50 | Critical for cooling |
| - | Thermal pads | Various | Various | 20 | $0.50-2.00 | For VRM, chipset |
| - | I/O Shield | Custom | Custom | 1 | $2-10 | Rear I/O panel |

### 16. Miscellaneous

| Item | Description | Part Number | Manufacturer | Qty | Unit Price | Notes |
|------|-------------|-------------|--------------|-----|------------|-------|
| - | Status LEDs | Various | Various | 5-10 | $0.05-0.20 | Power, HDD, debug |
| - | Switches | Various | Various | 2-3 | $0.10-0.50 | Power, reset, CMOS clear |
| - | Buzzer/Speaker | Various | Various | 1 | $0.20-1.00 | POST beep codes |

## Component Count Summary

| Category | Approximate Count |
|----------|------------------|
| ICs (Active) | 50-80 |
| Connectors | 30-50 |
| Resistors | 500-800 |
| Capacitors | 800-1200 |
| Inductors | 25-40 |
| Ferrite Beads | 20-40 |
| LEDs | 5-15 |
| Crystals/Oscillators | 2-5 |
| Mechanical | 20-40 |
| **Total Components** | **1500-2300** |

## Cost Estimate Summary

| Category | Cost Range (USD) |
|----------|------------------|
| Socket/Connectors | $30-60 |
| VRM Components | $150-300 |
| PCH Chipset | $40-80 |
| Power Delivery (misc) | $20-40 |
| I/O Controllers | $10-30 |
| USB-C Components | $5-15 |
| Audio/Network | $5-15 |
| Clock/BIOS | $5-10 |
| Passive Components | $100-200 |
| Mechanical/Heatsinks | $15-60 |
| PCB Fabrication | $500-2000 (prototype) |
| **Total (Prototype)** | **$880-2810** |

**Volume Production (100+ units):**
- Component costs: $300-600 per board
- PCB costs: $100-200 per board
- **Total: $400-800 per board**

## BOM Management

### Recommended Tools
1. **Excel/Google Sheets**: Simple BOM tracking
2. **KiCad BOM Plugin**: Auto-generate from schematic
3. **Arena PLM**: Product lifecycle management
4. **Altium 365**: Cloud BOM management

### BOM Fields
- **Item Number**: Unique identifier
- **Reference Designator**: U1, R1, C1, etc.
- **Quantity**: Number required
- **Manufacturer**: Component manufacturer
- **Manufacturer Part Number**: MPN
- **Supplier**: Digi-Key, Mouser, Arrow, etc.
- **Supplier Part Number**: SKU
- **Description**: Component description
- **Value/Rating**: Resistance, capacitance, voltage, etc.
- **Package/Footprint**: 0603, SOIC-8, BGA, etc.
- **Unit Price**: Cost per component
- **Extended Price**: Quantity × Unit Price
- **Lead Time**: Availability
- **Notes**: Special requirements

### Procurement Considerations

#### Component Availability
- Check stock at major distributors (Digi-Key, Mouser, Arrow)
- Have alternate components identified
- Long lead items: CPU socket, chipset, specialized ICs
- Plan for 3-6 month lead times on some components

#### Minimum Order Quantities (MOQ)
- Many components have MOQs (especially connectors)
- May need to order more than required
- Consider resale/storage of extras

#### Component Substitutions
- Always have 2-3 approved alternates
- Verify footprint compatibility
- Check electrical compatibility
- Re-validate if substituting

### Assembly Considerations

#### Manual vs. Automated Assembly
- **Prototype (1-5 boards)**: Hand assembly or small assembly house
- **Small production (10-100)**: Semi-automated assembly
- **Volume (100+)**: Fully automated pick-and-place

#### Assembly Files Required
1. **Centroid file**: X/Y positions of components
2. **BOM**: With complete part information
3. **Assembly drawing**: Component placement reference
4. **Pick-and-place file**: Machine-readable format

#### Component Packaging
- **Tape & Reel**: For automated assembly
- **Cut tape**: For prototyping
- **Tube**: For some ICs
- **Tray**: For large BGAs

## Critical Components Requiring Authorization

⚠️ **WARNING**: Some components require manufacturer authorization:

1. **Intel CPU Socket (LGA1700)**
   - May require authorization from Intel
   - Often available through authorized distributors
   - Check with Foxconn, Lotes

2. **Intel PCH Chipset**
   - **Requires NDA and authorization from Intel**
   - Only available to approved manufacturers
   - Alternative: Reference design or partnership

3. **Proprietary ICs**
   - Some VRM controllers may be allocated
   - May need to work with distributor/manufacturer

## Recommended Suppliers

### North America
- **Digi-Key**: Excellent stock, fast shipping
- **Mouser Electronics**: Large selection
- **Arrow Electronics**: Good for volume
- **Newark/Farnell**: Alternative source

### Asia
- **LCSC**: Cheap components, long shipping
- **Alibaba/Taobao**: Bulk purchasing

### Specialized
- **Octopart**: Component search engine
- **FindChips**: Price comparison
- **SnapEDA**: Footprint library

## Quality and Reliability

### Component Grades
- **Commercial**: 0°C to 70°C
- **Industrial**: -40°C to 85°C (recommended)
- **Automotive**: -40°C to 125°C
- **Military**: Extended specs

### Testing and Validation
- Source from authorized distributors to avoid counterfeits
- Consider counterfeit risk for critical components
- X-ray inspection for BGAs
- Functional testing of assembled boards

## Revision Control

### BOM Versioning
- Match BOM version to schematic/PCB version
- Track changes in revision history
- Note: This is BOM v1.0 for initial design

### Change Management
- Document all component changes
- Re-validate with substitutions
- Update assembly files

## Notes for Procurement

1. **Lead Time Planning**: Order long-lead items early (CPU socket, chipset)
2. **Inventory Management**: Keep critical components in stock
3. **Cost Optimization**: Negotiate pricing for volume orders
4. **Obsolescence**: Monitor component lifecycles, plan for EOL parts
5. **Quality**: Buy from authorized distributors only

This BOM template provides a framework for the complete motherboard design. Actual implementation will require detailed component selection, validation, and procurement planning.
