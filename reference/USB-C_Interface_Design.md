# USB Type-C Interface Design

## Overview
USB Type-C is a versatile connector supporting multiple protocols including USB 2.0, USB 3.2, USB4, DisplayPort, Thunderbolt, and Power Delivery. This guide covers implementing a USB-C port for an LGA1700 motherboard.

## USB-C Capabilities

### Protocol Support Options

1. **USB 2.0 Only** (Simplest)
   - Data: Up to 480 Mbps
   - Power: Up to 15W (5V @ 3A)
   - Pins required: 8 (minimal)

2. **USB 3.2 Gen 2** (Recommended minimum)
   - Data: Up to 10 Gbps (USB 3.2 Gen 2)
   - Power: Up to 15W standard, 100W with PD
   - Pins required: 16+

3. **USB 3.2 Gen 2x2**
   - Data: Up to 20 Gbps
   - Power: Up to 100W with PD
   - Requires both TX/RX pairs

4. **USB4 / Thunderbolt 4** (Most complex)
   - Data: Up to 40 Gbps
   - Requires USB4/TB4 controller
   - Beyond scope of basic implementation

### Recommended for This Project
**USB 3.2 Gen 2 (10 Gbps) with USB Power Delivery**
- Good balance of capability and complexity
- Widely compatible
- Future-proof for most use cases

## USB Type-C Connector Pinout

### 24-Pin Connector Layout

```
Top Row (12 pins): A1-A12
Bottom Row (12 pins): B1-B12 (flipped mirror)
```

#### Pin Functions

| Pin | Signal | Description |
|-----|--------|-------------|
| A1, B1, A12, B12 | GND | Ground |
| A2, B2, A11, B11 | TX+ / TX- | SuperSpeed differential pair (USB 3.x) |
| A3, B3, A10, B10 | RX+ / RX- | SuperSpeed differential pair (USB 3.x) |
| A4, B4, A9, B9 | VBUS | Power (5V, 9V, 12V, 15V, or 20V) |
| A5, B5 | CC1, CC2 | Configuration Channel |
| A6, B6 | D+, D+ | USB 2.0 Data Plus |
| A7, B7 | D-, D- | USB 2.0 Data Minus |
| A8, B8 | SBU1, SBU2 | Sideband Use (Alternate Mode) |

#### Symmetric Design
- USB-C is reversible
- Connector can be inserted either way
- CC pins detect orientation
- TX/RX pairs swapped by orientation

## Signal Groups

### 1. USB 2.0 Signals (D+/D-)

#### Characteristics
- Differential pair
- Data rate: Up to 480 Mbps (Hi-Speed USB)
- Impedance: 90 ohms differential
- Voltage: 3.3V logic levels

#### Routing
- Keep differential pair matched (±2mm tolerance)
- 90-ohm differential impedance
- Avoid sharp bends
- Keep away from noisy signals

### 2. USB 3.2 SuperSpeed Signals

#### TX Pair (Transmit from host)
- TX+ and TX- (differential)
- Data rate: 10 Gbps (Gen 2)
- Impedance: 90 ohms differential
- AC-coupled

#### RX Pair (Receive to host)
- RX+ and RX- (differential)
- Data rate: 10 Gbps (Gen 2)
- Impedance: 90 ohms differential
- AC-coupled

#### Routing Requirements
- **Length matching critical**: ±0.2mm (±8 mils) within pair
- **90-ohm differential impedance**
- **Minimize vias**: Each via adds loss
- **Avoid sharp bends**: 45° minimum angle
- **Isolation**: 3× trace width from other signals
- **AC coupling**: 0.1µF capacitors in series (typically in connector or controller)

### 3. Configuration Channel (CC)

#### CC1 and CC2 Pins
- Used for cable detection
- Orientation detection
- Power role determination (DFP, UFP, DRP)
- USB PD communication

#### Pull-up/Pull-down Resistors
- **DFP (Downstream Facing Port - Host)**:
  - Rp pull-up to VBUS or 5V
  - Rp value indicates power capability:
    - 56kΩ: Default USB power (500mA/900mA)
    - 22kΩ: 1.5A @ 5V
    - 10kΩ: 3A @ 5V

- **UFP (Upstream Facing Port - Device)**:
  - Rd pull-down to ground
  - Typically 5.1kΩ

- **DRP (Dual-Role Port)**:
  - Can be DFP or UFP
  - Toggles Rp/Rd to negotiate role

#### For Motherboard (DFP - Host)
- Apply Rp (10kΩ for 3A capability) to both CC1 and CC2
- Monitor CC pins for device connection
- CC with higher voltage indicates cable orientation

### 4. VBUS (Power)

#### Standard USB Power
- 5V @ 0.5A to 3A (default, no PD)
- Controlled by power switch
- Overcurrent protection required

#### USB Power Delivery (Optional)
- Voltages: 5V, 9V, 12V, 15V, 20V
- Current: Up to 5A
- Max power: 100W (20V @ 5A)
- Requires PD controller IC
- Negotiated via CC pins

### 5. SBU (Sideband Use)

#### Purpose
- Used for Alternate Modes (DisplayPort, Thunderbolt, etc.)
- Not used for standard USB
- Can be left unconnected for basic USB implementation

## System Integration

### Connection to CPU/Chipset

#### Intel LGA1700 Platform
- USB 3.2 Gen 2 ports typically from:
  - PCH (Platform Controller Hub) - Z690/Z790 chipset
  - Some lanes directly from CPU (varies)

#### PCH Provides
- Multiple USB 3.2 Gen 2 ports (typically 4-8)
- USB 2.0 ports (10-14)
- Integrated USB hub functionality

### Signal Routing
```
PCH → USB 3.2 Hub (optional) → USB-C Connector
PCH → USB 2.0 Hub (optional) → USB-C Connector
```

## USB Power Delivery Implementation

### Basic Implementation (No PD)
Simply provide 5V on VBUS with current limiting.

### With USB PD Controller

#### Popular PD Controllers
- **TI (Texas Instruments)**:
  - TPS65987D (Complete PD solution)
  - TPS25750 (Simplified)

- **Cypress/Infineon**:
  - CYPD3177 (PD controller + gate driver)
  - CYPD4225 (Dual-port)

- **On Semiconductor**:
  - FUSB302 (PD PHY only, requires MCU)

#### PD Controller Functions
- Monitors CC pins
- Handles PD negotiation protocol
- Controls VBUS voltage via external DC-DC
- Provides load switch control
- Fault protection

### Power Path

```
Main 5V → PD Controller → VBUS Switch → USB-C VBUS
                ↓
           Buck-Boost Converter (for 9V/12V/15V/20V)
```

#### Components Needed
1. PD Controller IC
2. Load switch / Power MOSFET for VBUS
3. Current sense resistor
4. Buck-boost converter (if supporting >5V)
5. Protection circuits

## PCB Layout Guidelines

### Layer Stackup
- USB 3.2 signals on inner layers preferred
- Reference to solid ground plane
- Avoid plane splits under signals

### Trace Specifications

#### USB 2.0 (D+/D-)
- Differential impedance: 90 ohms
- Trace width: ~0.2mm (depends on stackup)
- Spacing: ~0.2mm
- Length match: ±2mm

#### USB 3.2 Gen 2 (TX/RX)
- Differential impedance: 90 ohms
- Trace width: ~0.15mm (depends on stackup)
- Spacing: ~0.15mm
- Length match: ±0.2mm (±8 mils)
- Keep pairs together

### Critical Layout Rules

1. **Reference Plane**
   - Continuous ground plane under signals
   - No voids or splits
   - Via stitching along signal path

2. **Via Management**
   - Minimize via count (each adds loss)
   - Use blind/buried vias if possible
   - Remove via stubs (back-drilling)

3. **Isolation**
   - Keep USB 3.2 away from USB 2.0 (3W minimum)
   - Shield from other high-speed signals
   - Ground guard traces if needed

4. **Length Matching**
   - TX+/TX- matched to ±0.2mm
   - RX+/RX- matched to ±0.2mm
   - D+/D- matched to ±2mm

5. **ESD Protection**
   - ESD diodes on all USB signals
   - Place close to connector
   - Low capacitance (< 0.5pF per line for USB 3.2)

### Connector Placement
- Accessible from rear I/O or front panel
- Proper mechanical support
- Adequate clearance for cable insertion
- Consider EMI shielding (metal shell grounded)

## Component Selection

### USB Type-C Connector
- 24-pin receptacle
- Mid-mount or through-hole
- Rated for insertion cycles (10,000+ typical)
- Examples:
  - JAE DX07S024JJ1R1500
  - Molex 2171750001
  - TE Connectivity 2199230-4

### ESD Protection
- Multi-channel TVS diode array
- Low capacitance for USB 3.2 (< 0.5pF)
- Examples:
  - TI TPD8S009
  - NXP IP4292CZ10
  - ON Semi NUP4202

### Power Switch
- Load switch for VBUS
- Overcurrent limiting
- Low Rds(on)
- Examples:
  - TI TPS2553
  - TI TPS2561

### DC-DC Converter (for PD voltages)
- Buck-boost topology
- Input: 5V-20V
- Output: 5V, 9V, 12V, 15V, 20V (selectable)
- Controlled by PD controller

## Decoupling and Filtering

### VBUS Capacitance
- 4.7µF to 47µF ceramic (X7R)
- Voltage rating: 25V (for 20V PD)
- Low ESR
- Placed close to connector

### USB Controller Decoupling
- 10µF + 0.1µF per power pin
- Close to IC

### Ferrite Beads
- On VBUS (optional, for EMI)
- On power rails to USB controller
- Select based on frequency and current

## Testing and Validation

### Electrical Tests

1. **Signal Integrity**
   - Eye diagram measurement
   - Jitter analysis
   - Compliance testing (USB-IF tools)

2. **Power Delivery**
   - Verify voltage levels (5V, 9V, etc.)
   - Check current limiting
   - PD negotiation test

3. **ESD Testing**
   - IEC 61000-4-2 (±8kV contact, ±15kV air)
   - USB specification ESD requirements

4. **Interoperability**
   - Test with various devices
   - Different cables (USB 2.0, 3.2)
   - PD chargers and power banks

### USB-IF Compliance
- Full compliance requires USB-IF testing
- Certified labs perform official tests
- Can use self-testing tools for development

## Common Issues and Solutions

### USB 3.2 Not Detected
- Check differential impedance
- Verify AC coupling capacitors
- Check for ground plane continuity
- Verify controller configuration

### Intermittent Connection
- Poor signal integrity (impedance mismatch)
- Connector contact issues
- Cable quality
- EMI interference

### PD Negotiation Fails
- CC pull-up resistor incorrect value
- PD controller configuration
- Firmware issues
- Voltage levels incorrect

### High Error Rate
- Excessive jitter
- Crosstalk from nearby signals
- Inadequate return path
- Via stubs causing reflections

## Design Checklist

- [ ] Connector selected and footprint added
- [ ] USB controller/PCH connections identified
- [ ] ESD protection designed
- [ ] Power delivery method chosen (5V only or PD)
- [ ] PD controller selected (if using PD)
- [ ] CC pull-up resistors calculated
- [ ] Differential impedance calculated for stackup
- [ ] USB 3.2 TX/RX routing completed with length matching
- [ ] USB 2.0 D+/D- routing completed
- [ ] VBUS power path designed
- [ ] Overcurrent protection implemented
- [ ] Decoupling capacitors placed
- [ ] Ground plane continuous under signals
- [ ] Via count minimized
- [ ] ESD protection placed near connector
- [ ] Mechanical clearances verified
- [ ] Mounting holes for connector support

## Component Count Estimate

- USB-C connector: 1
- ESD protection IC: 1
- Load switch: 1
- PD controller: 1 (if using PD)
- DC-DC converter: 1 (if using PD)
- Resistors: 10-20
- Capacitors: 20-40
- Ferrite beads: 2-4
- MOSFETs/diodes: 2-4

## Cost Estimate (Per USB-C Port)

- Connector: $0.50-2.00
- ESD protection: $0.30-1.00
- Load switch: $0.20-0.80
- PD controller: $1.50-4.00 (if used)
- DC-DC converter: $1.00-3.00 (if used)
- Passives: $0.50-1.00

**Total per port:**
- Basic (5V only): $2-5
- With PD: $5-12

## Advanced Features (Optional)

### DisplayPort Alternate Mode
- Allows DisplayPort over USB-C
- Uses SBU pins and TX/RX lanes
- Requires mux/switch IC
- Significantly more complex

### Thunderbolt 4
- 40 Gbps bidirectional
- Requires Intel/Apple controller
- Very complex integration
- Expensive certification

### USB4
- Next generation, 40 Gbps
- Compatible with Thunderbolt 3/4
- Requires dedicated controller
- Still emerging

## Reference Documents

1. USB Type-C Specification 2.1
2. USB 3.2 Specification
3. USB Power Delivery Specification 3.1
4. PCH Datasheet (Z690/Z790)
5. Connector manufacturer datasheets
6. PD controller IC datasheets
7. USB-IF compliance documentation

## Recommended Tools

- Differential impedance calculator
- USB protocol analyzer (for debugging)
- Oscilloscope with high bandwidth (10+ GHz for Gen 2)
- USB-IF test tools (for compliance)

## Additional Considerations

### Multiple Ports
- Each port needs separate CC resistors
- Shared VBUS possible with current sharing
- PD controller can handle multiple ports

### Front Panel Headers
- Can route to front panel connector
- Maintain signal integrity over longer distances
- Use shielded cable if possible

### Firmware/Software
- PD controller may need firmware
- OS drivers for USB controller
- BIOS/UEFI configuration

This USB-C interface design provides a solid foundation for implementing a modern, capable USB port on your motherboard with optional power delivery support.
