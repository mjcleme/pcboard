# Intel LGA1700 Socket Specifications

## Overview
LGA1700 (Land Grid Array 1700) is Intel's socket for 12th, 13th, and 14th generation Core processors (Alder Lake, Raptor Lake, and Raptor Lake Refresh).

## Physical Specifications

### Socket Dimensions
- Pin count: 1700 pins
- Socket dimensions: 45mm x 37.5mm (rectangular, not square)
- Keep-out zone: Must account for cooler mounting mechanism
- Z-height: Varies by loading mechanism design

### Mounting
- Uses ILM (Independent Loading Mechanism)
- 4-point mounting system
- Backplate required for cooler mounting
- Torque specifications critical for proper contact

## Pin Configuration

### Power Pins (Approximate Distribution)
- VCC (Core power): ~200+ pins
- VSS (Ground): ~600+ pins
- VCCIN: ~100+ pins
- VCCSA (System Agent): ~50+ pins
- VDD (Memory I/O): ~50+ pins
- VCC_GT (Graphics): ~30+ pins
- 3.3V, 1.8V, 1.05V auxiliary rails

### Signal Pins
- DDR5 memory interface: ~400 pins
  - 2 channels, 64-bit per channel
  - Command, address, data, DQS strobe signals
- PCIe lanes: ~200 pins
  - Up to 20 PCIe lanes from CPU (16+4 typical)
  - PCIe Gen 5.0 support (32 GT/s)
- DMI (Direct Media Interface): Connects to PCH chipset
  - PCIe Gen 4.0 x8 or Gen 5.0 x4
- USB, SATA, and other I/O through PCH

### Clock Pins
- Reference clocks (100MHz typical)
- Spread spectrum clocking support
- Multiple clock domains

## Electrical Specifications

### Power Requirements
- Maximum TDP: Up to 241W (varies by CPU model)
  - i9-14900K: 125W base, 253W turbo
  - i9-13900K: 125W base, 253W turbo
  - i9-12900K: 125W base, 241W turbo

### Voltage Rails
1. **VCCIN** (Input voltage)
   - Nominal: 1.8V
   - Range: 1.5V - 2.0V
   - Current: Up to 100A+

2. **VCC** (Core voltage)
   - Nominal: 0.6V - 1.5V (dynamic, varies by load)
   - Current: Up to 300A+ during peak load
   - Requires VR13.5/VR14 compliant VRM

3. **VCCSA** (System Agent)
   - Nominal: ~1.05V
   - Current: Up to 10A

4. **VDD** (Memory I/O)
   - DDR5: 1.1V nominal
   - Current: Up to 20A per channel

5. **VCC_GT** (Graphics)
   - Nominal: ~1.05V
   - Current: Up to 30A

### Signal Characteristics
- DDR5 signals:
  - Single-ended SSTL (Stub Series Terminated Logic)
  - Voltage: 0.55V (VDD/2)
  - Impedance: 40-ohm differential for DQ/DQS

- PCIe Gen 5:
  - Differential signaling
  - 100-ohm differential impedance
  - Data rate: 32 GT/s (16 GHz)

## Thermal Specifications

### Junction Temperature
- Tjmax: 100°C typical (varies by model)
- Throttling begins before Tjmax

### Cooling Requirements
- Adequate heat dissipation required
- IHS (Integrated Heat Spreader) present
- Thermal interface material (TIM) needed
- Cooler must cover entire IHS area

## Memory Interface (DDR5)

### Configuration
- 2 channels, 64-bit per channel (128-bit total)
- Up to 2 DIMMs per channel (4 total)
- Maximum capacity: 128GB (32GB per DIMM)

### Speeds
- JEDEC standard: DDR5-4800, DDR5-5600
- Overclocking: Up to DDR5-7200+ with proper design

### Signal Groups per Channel
- DQ (Data): 64 bits
- DQS (Data Strobe): 8 pairs (differential)
- DM (Data Mask): 8 signals
- CA (Command/Address): ~15 signals
- CK (Clock): 1 pair (differential)
- Control signals: CS, CKE, ODT, etc.

### Routing Requirements
- Length matching critical:
  - DQ/DQS within byte lane: ±0.2mm (±8 mils)
  - DQS-to-DQ: ±0.3mm (±12 mils)
  - DQS-to-CK: ±1.0mm (±40 mils)
  - CA signals: ±0.5mm (±20 mils)
- Controlled impedance: 40-ohm differential
- Via minimization on signal paths
- Serpentine routing for length matching

## PCIe Interface

### Lane Distribution (Example)
- Slot 1 (x16 electrical): PCIe Gen 5.0 x16
- M.2 slot: PCIe Gen 4.0 x4
- Additional lanes may go to PCH

### Signal Characteristics
- Differential pairs: TX+/TX-, RX+/RX-
- 100-ohm differential impedance
- AC coupling required
- Reference clock: 100MHz

### Routing Requirements
- Length matching: ±5mm within differential pair
- Minimize vias and stubs
- No sharp bends (45° minimum)
- Keep away from noisy signals

## PCH (Platform Controller Hub) Interface

### DMI (Direct Media Interface)
- PCIe-based link to chipset
- Gen 4.0 x8 or Gen 5.0 x4
- Same electrical characteristics as PCIe

### Chipset Selection
Common chipsets for LGA1700:
- Z790/Z690: Enthusiast/Overclocking
- H770/H670: Mainstream
- B760/B660: Value

### Chipset Provides
- Additional USB ports (USB 2.0, 3.2)
- SATA ports (typically 4-8)
- Additional PCIe lanes
- Audio codec interface
- Network interface
- SPI for BIOS/UEFI

## Clock Generation

### Requirements
- Main system clock: 100MHz
- Multiple clock domains
- Low jitter (<1ps RMS)
- Spread spectrum support

### Typical Components
- Clock generator IC (e.g., IDT/Renesas)
- Crystal oscillator
- Clock distribution buffers

## Power Sequencing

### Critical Sequence
1. Standby power (3.3V_SB, 5V_SB)
2. VCCIN
3. VCC, VCCSA, VDD, VCC_GT (can be simultaneous)
4. Assert power good signals
5. Release reset

### Monitoring
- Power good signals
- Over-current protection
- Over-voltage protection
- Thermal monitoring

## Design Considerations

### Decoupling
- Multiple capacitors per power rail
- Various capacitor values (0.1µF, 1µF, 10µF, 100µF)
- Placed as close to socket as possible
- Different dielectrics (X7R, X5R for bulk)

### Grounding
- Solid ground planes
- Via stitching around socket perimeter
- Star-point grounding for analog sections
- Separate analog/digital grounds where applicable

### EMI/EMC
- Proper shielding considerations
- Trace routing to minimize emissions
- Ferrite beads on appropriate signals
- Clock signal containment

## Manufacturing Considerations

### PCB Requirements
- High-quality FR-4 or better
- Controlled dielectric constant (Dk)
- Controlled loss tangent
- Thickness tolerance: ±10%
- Copper weight: 1-2oz for signal, 2oz+ for power

### Assembly
- Pick-and-place accuracy critical
- Socket placement tolerance: ±0.1mm
- Reflow profile for components
- Inspection (AOI, X-ray for BGA components)

## Testing and Validation

### Required Tests
1. Power-on self-test
2. Memory training and stability
3. PCIe link training
4. Thermal validation
5. Power consumption measurement
6. EMI/EMC compliance

### Debug Features
- POST code display
- Debug LEDs
- JTAG/Debug headers
- Voltage test points

## Reference Documents Needed

**Note**: Most detailed specifications require Intel NDA

1. LGA1700 Socket Mechanical Specification
2. 12th/13th/14th Gen Core Processor Datasheet
3. Platform Design Guide (PDG)
4. Voltage Regulator Down (VRD) 13.5/14 Design Guide
5. DDR5 Memory Design Guide
6. PCIe Gen 5.0 Specification
7. Chipset Datasheet (Z790/Z690/etc.)

## Common Pitfalls

1. **Insufficient decoupling** - Results in instability
2. **Poor DDR5 routing** - Memory won't train or will be unstable
3. **Inadequate VRM** - CPU throttling or shutdown
4. **Thermal issues** - Overheating, reduced performance
5. **Impedance mismatch** - Signal integrity issues
6. **Power sequencing errors** - Damage to CPU or components

## Estimated Component Count

- Resistors: 300-500
- Capacitors: 400-700
- Inductors: 20-40
- ICs: 30-50
- Connectors: 15-30
- Socket: 1 (LGA1700)
- MOSFETs: 40-60 (for VRM)
- Diodes: 20-40

## Cost Estimate

- PCB fabrication: $500-2000 (prototype)
- Components: $200-500 (depending on choices)
- Assembly: $300-1000 (if using service)
- Testing equipment: Variable

**Total estimated cost for prototype: $1000-3500**
