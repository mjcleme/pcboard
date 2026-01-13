# PCB Layer Stackup Design

## Overview
A proper layer stackup is critical for signal integrity, power delivery, and manufacturability of a high-speed motherboard. This document defines the recommended stackup for the Intel i9 LGA1700 motherboard.

## Recommended: 10-Layer Stackup

This provides an excellent balance of signal integrity, power delivery, and cost.

### Layer Configuration

```
Layer 1  (Top)        - Signal / Components
Layer 2  (Inner)      - Ground Plane (GND)
Layer 3  (Inner)      - High-Speed Signals (DDR5, USB 3.2, PCIe)
Layer 4  (Inner)      - Power Plane (VCC, VCCIN - CPU cores)
Layer 5  (Inner)      - Signal / Mixed
Layer 6  (Inner)      - Signal / Mixed
Layer 7  (Inner)      - Power Plane (3.3V, 5V, 1.1V, VCCSA)
Layer 8  (Inner)      - High-Speed Signals (DDR5, PCIe)
Layer 9  (Inner)      - Ground Plane (GND)
Layer 10 (Bottom)     - Signal / Components
```

### Detailed Layer Descriptions

#### Layer 1 - Top Signal/Components
- **Type**: Signal + Component layer
- **Copper Weight**: 1 oz (35 µm)
- **Usage**:
  - Component placement (CPU socket, DIMM slots, VRM, etc.)
  - Signal routing (low-speed signals, power traces)
  - VRM power traces (may use 2oz locally)
  - Silkscreen and solder mask

#### Layer 2 - Ground Plane
- **Type**: Continuous ground plane
- **Copper Weight**: 1 oz (35 µm)
- **Usage**:
  - Solid ground reference for Layer 1 and Layer 3
  - Return path for high-frequency currents
  - Shielding between layers
  - Via stitching throughout
  - Avoid splits or voids under high-speed signals

#### Layer 3 - High-Speed Signal Layer
- **Type**: Signal routing layer
- **Copper Weight**: 0.5 oz (17 µm) or 1 oz
- **Usage**:
  - DDR5 data/strobe/address signals (primary)
  - USB 3.2 differential pairs
  - PCIe lanes (Gen 4/5)
  - Critical high-speed signals
- **Reference Plane**: Layer 2 (GND) above, Layer 4 below
- **Impedance Control**: Critical - 40Ω/80Ω differential

#### Layer 4 - Power Plane (CPU Cores)
- **Type**: Power plane
- **Copper Weight**: 2 oz (70 µm)
- **Usage**:
  - VCC (CPU core voltage, 0.6-1.5V)
  - VCCIN (CPU input voltage, 1.8V)
  - Split planes with adequate clearance
  - Low impedance distribution
  - Via stitching between splits

#### Layer 5 - Signal Layer (Mixed)
- **Type**: Signal routing layer
- **Copper Weight**: 0.5 oz or 1 oz
- **Usage**:
  - Medium-speed signals
  - Additional routing for high-speed if needed
  - Clock signals
  - Control signals
- **Reference Plane**: Layer 4 (Power) above, Layer 6 or 7 below

#### Layer 6 - Signal Layer (Mixed)
- **Type**: Signal routing layer
- **Copper Weight**: 0.5 oz or 1 oz
- **Usage**:
  - Similar to Layer 5
  - Medium-speed signals
  - Perpendicular routing to Layer 5 (if possible)
- **Reference Plane**: Layer 5 above, Layer 7 (Power) below

#### Layer 7 - Power Plane (Peripheral Rails)
- **Type**: Power plane
- **Copper Weight**: 2 oz (70 µm)
- **Usage**:
  - 3.3V rail
  - 5V rail
  - 1.1V (DDR5 VDD)
  - VCCSA (System Agent)
  - VCC_GT (Graphics)
  - Split planes with clearance

#### Layer 8 - High-Speed Signal Layer
- **Type**: Signal routing layer
- **Copper Weight**: 0.5 oz or 1 oz
- **Usage**:
  - DDR5 signals (secondary routing)
  - PCIe lanes
  - Additional high-speed signals
  - Return routing for Layer 3 signals
- **Reference Plane**: Layer 7 (Power) above, Layer 9 (GND) below
- **Impedance Control**: Critical

#### Layer 9 - Ground Plane
- **Type**: Continuous ground plane
- **Copper Weight**: 1 oz (35 µm)
- **Usage**:
  - Solid ground reference for Layer 8 and Layer 10
  - Return path
  - Shielding
  - No splits or voids under high-speed signals

#### Layer 10 - Bottom Signal/Components
- **Type**: Signal + Component layer
- **Copper Weight**: 1 oz (35 µm)
- **Usage**:
  - Component placement (back side)
  - Signal routing
  - Connectors, passive components
  - Silkscreen and solder mask

## Stackup Parameters

### Total Board Thickness
- **Standard**: 1.6mm (62.99 mils)
- Alternative: 1.2mm or 2.0mm depending on requirements

### Dielectric Material
- **Type**: FR-4 (standard) or High-Tg FR-4
- **Properties**:
  - Dk (Dielectric Constant): 4.3-4.5 @ 1 MHz (typical)
  - Dk @ high frequency: ~4.0 @ 1 GHz
  - Df (Dissipation Factor): < 0.02
  - Tg (Glass Transition): > 170°C (High-Tg preferred)

### Prepreg and Core Thicknesses (Example for 1.6mm total)

```
Layer 1:    Copper 1 oz (35 µm)
            ↓
            Prepreg 2116 (114 µm) - Forms 127 µm dielectric after pressing
            ↓
Layer 2:    Copper 1 oz (35 µm)
            ↓
            Core 10 mil (254 µm total with copper)
            ↓
Layer 3:    Copper 0.5 oz (17 µm)
            ↓
            Prepreg 2116 (114 µm)
            ↓
Layer 4:    Copper 2 oz (70 µm)
            ↓
            Core 15 mil (381 µm total)
            ↓
Layer 5:    Copper 1 oz (35 µm)
            ↓
            Core 10 mil (254 µm total)
            ↓
Layer 6:    Copper 1 oz (35 µm)
            ↓
            Prepreg 2116 (114 µm)
            ↓
Layer 7:    Copper 2 oz (70 µm)
            ↓
            Core 10 mil (254 µm total)
            ↓
Layer 8:    Copper 0.5 oz (17 µm)
            ↓
            Prepreg 2116 (114 µm)
            ↓
Layer 9:    Copper 1 oz (35 µm)
            ↓
            Prepreg 2116 (114 µm)
            ↓
Layer 10:   Copper 1 oz (35 µm)

Total: ~1.6mm (varies with pressing)
```

## Impedance Calculations

### Critical Impedances

#### Single-Ended 40Ω (DDR5 DQ, DM)
- Layer 3 referenced to Layer 2 (GND)
- Trace width: ~0.127mm (5 mil)
- Dielectric height: ~127 µm (5 mil)
- Dk: 4.3

#### Differential 80Ω (DDR5 DQS, CK)
- Layer 3 referenced to Layer 2 (GND)
- Trace width: ~0.127mm (5 mil) each
- Spacing: ~0.127mm (5 mil)
- Dielectric height: ~127 µm (5 mil)
- Dk: 4.3

#### Differential 90Ω (USB 3.2, PCIe Gen 4/5)
- Layer 3 referenced to Layer 2 (GND)
- Trace width: ~0.15mm (6 mil) each
- Spacing: ~0.15mm (6 mil)
- Dielectric height: ~127 µm (5 mil)
- Dk: 4.3

#### Differential 100Ω (PCIe - alternative)
- Layer 3 referenced to Layer 2 (GND)
- Trace width: ~0.15mm (6 mil) each
- Spacing: ~0.18mm (7 mil)
- Dielectric height: ~127 µm (5 mil)
- Dk: 4.3

**Note**: Exact values depend on manufacturer's stackup. Always verify with impedance calculator using actual stackup parameters.

## Design Rules

### Minimum Track Width
- Signal layers: 0.1mm (4 mil)
- Power traces (< 1A): 0.2mm (8 mil)
- Power traces (1-5A): 0.5-1.0mm
- Power traces (> 5A): 1.5mm+

### Minimum Spacing
- Signal to signal: 0.1mm (4 mil)
- Signal to power/ground: 0.15mm (6 mil)
- High voltage clearance: 0.3mm+ depending on voltage

### Via Specifications

#### Standard Via
- Finished hole: 0.2-0.3mm (8-12 mil)
- Pad diameter: 0.4-0.6mm (16-24 mil)
- Annular ring: 0.1mm (4 mil) minimum

#### Microvia (if used)
- Finished hole: 0.1-0.15mm (4-6 mil)
- Pad diameter: 0.2-0.3mm (8-12 mil)
- Span: 1-2 layers only

#### Thermal Via (under components)
- Array of vias for heat transfer
- Typical: 0.3mm hole, 0.8mm spacing
- Filled or tented

#### Via-in-Pad (if needed)
- Via filled with epoxy/copper
- Allows BGA placement over via
- More expensive process

### Drill Specifications
- Minimum drill: 0.2mm (8 mil) typical
- Maximum aspect ratio: 10:1 (board thickness : hole diameter)
- For 1.6mm board: minimum drill ~0.16mm, but 0.2mm more practical

## Power and Ground Plane Design

### Plane Splitting
- Minimize splits
- Avoid splits under high-speed signals
- Adequate clearance between different voltages (0.5mm+)
- Via stitching along split boundaries (every 5-10mm)

### Decoupling Strategy
- Place capacitors on both top and bottom near socket
- Multiple vias per capacitor to planes
- Short, wide traces
- Distributed throughout power planes

### Return Path
- Keep signal return path continuous
- Via stitching when crossing plane gaps
- Minimize loop area for all signals

## Thermal Considerations

### Copper Weight Distribution
- Heavier copper (2oz) in power planes helps heat dissipation
- Heavier copper locally under VRM components
- Balance electrical and thermal needs

### Thermal Vias
- Under CPU socket area
- Under VRM components
- Under any high-power component
- Typical array: 0.3mm vias at 0.8-1.0mm pitch

## Manufacturing Considerations

### Fabrication Class
- IPC Class 2 or 3
- Class 3 for better reliability (space/military grade)

### Tolerances
- Track width: ±10%
- Spacing: ±10%
- Impedance: ±10% (tighter with controlled impedance service)
- Layer-to-layer registration: ±0.075mm (3 mil)

### Surface Finish
- **ENIG (Electroless Nickel Immersion Gold)**: Recommended
  - Flat surface for fine-pitch components
  - Good shelf life
  - Suitable for wire bonding
- Alternatives: HASL, OSP, Immersion Silver

### Solder Mask
- Color: Typically green, black (professional look)
- Clearance: 0.05mm (2 mil) from pads
- Defined masks for better registration

### Silkscreen
- Color: White (on dark mask) or black (on light mask)
- Component designators
- Polarity marks
- Test points
- Version/revision info
- Manufacturer info

## Cost Considerations

### Factors Affecting Cost
1. **Layer count**: 10 layers is expensive but manageable
2. **Board size**: Larger = more expensive
3. **Copper weight**: Heavier copper costs more
4. **Surface finish**: ENIG more expensive than HASL
5. **Minimum features**: Smaller holes/traces = higher cost
6. **Controlled impedance**: Add-on service cost
7. **Quantity**: Prototypes expensive, production cheaper

### Estimated PCB Cost
- **Prototype (1-5 boards)**: $500-2000
  - Large size (ATX: ~305mm x 244mm)
  - 10 layers
  - Mixed copper weights
  - Controlled impedance
  - ENIG finish
  - Lead time: 2-4 weeks

- **Small production (10-50 boards)**: $200-500 per board
- **Volume production (100+)**: $100-200 per board

## Manufacturer Selection

### Recommended Capabilities
- 10+ layer capability
- Controlled impedance service
- HDI (if using microvias)
- 2oz copper minimum
- 4-6 mil min trace/space
- Good registration tolerances
- ENIG surface finish
- Quality certifications (ISO 9001, UL, etc.)

### Example Manufacturers
- **High-end**: TTM Technologies, Elmatica, Advanced Circuits
- **Mid-range**: Sierra Circuits, Sunstone Circuits, Bay Area Circuits
- **Overseas**: JLCPCB (advanced service), PCBWay, Seeed Studio
  - Cheaper but longer lead times
  - Quality varies

## Testing and Validation

### Electrical Testing
- Flying probe test (prototypes)
- Bed-of-nails fixture (production)
- 100% continuity and isolation

### Impedance Testing
- TDR (Time Domain Reflectometry)
- Coupon testing
- Verify target impedances achieved

### Microsection Analysis
- Cross-section of board
- Verify layer stack as designed
- Check hole plating quality
- Optional but recommended for first article

## Documentation for Manufacturer

### Required Files
1. **Gerber files** (RS-274X format)
   - One file per layer
   - Solder mask top/bottom
   - Silkscreen top/bottom
   - Paste mask (if needed)

2. **Drill files** (Excellon format)
   - Through-hole drill
   - Buried/blind vias (if used)

3. **Fab drawing**
   - Board outline
   - Dimensions
   - Drill table
   - Layer stackup
   - Notes and special requirements

4. **IPC-2581 or ODB++** (optional, comprehensive format)

5. **Assembly files** (if using assembly service)
   - BOM (Bill of Materials)
   - Centroid/Pick-and-place file
   - Assembly drawing

### Stackup Documentation
Provide detailed stackup table:
- Layer number and name
- Copper weight per layer
- Dielectric type and thickness
- Total thickness
- Impedance requirements

## Alternative Stackup Options

### 8-Layer Stackup (Cost Reduction)
```
1. Top Signal
2. Ground
3. Signal (High-speed)
4. Power
5. Power
6. Signal (High-speed)
7. Ground
8. Bottom Signal
```
- Less routing space
- Tighter design
- Lower cost

### 12-Layer Stackup (Premium)
```
1. Top Signal
2. Ground
3. Signal (DDR5)
4. Power (VCC/VCCIN)
5. Ground
6. Signal (PCIe)
7. Signal (General)
8. Ground
9. Power (3.3V, 5V, etc.)
10. Signal (DDR5 return)
11. Ground
12. Bottom Signal
```
- More routing layers
- Better signal integrity
- Higher cost
- More ground layers for shielding

## Summary

This 10-layer stackup provides:
- Excellent signal integrity for DDR5 and PCIe Gen 5
- Adequate power plane distribution
- Controlled impedance routing
- Manufacturable at reasonable cost
- Good thermal management
- Industry-standard stackup approach

It represents a professional-grade design suitable for a high-performance motherboard.
