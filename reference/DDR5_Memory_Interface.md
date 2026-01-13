# DDR5 Memory Interface Design

## Overview
DDR5 (Double Data Rate 5) is the latest generation of memory technology, offering higher speeds, increased capacity, and improved power efficiency compared to DDR4.

## DDR5 Key Features

### Improvements over DDR4
- Higher speeds: DDR5-4800 to DDR5-8400+ (vs DDR4-2133 to DDR4-3200)
- Increased capacity: Up to 64GB per DIMM (vs 32GB for DDR4)
- Lower voltage: 1.1V (vs 1.2V for DDR4)
- On-die ECC (Error Correction Code)
- Improved signal integrity
- Power management IC (PMIC) on DIMM

### Data Rates
- JEDEC Standard: DDR5-4800, DDR5-5600, DDR5-6400
- XMP/EXPO profiles: DDR5-6000 to DDR5-8400+
- Data rate (MT/s) = 2 × Memory Clock (MHz)

## LGA1700 Memory Configuration

### Channel Configuration
- 2 memory channels (A and B)
- 64 bits per channel
- 128-bit total bus width
- Up to 2 DIMMs per channel (4 DIMMs total)

### Topology Options

#### 1T (1 DIMM per channel)
- Best signal integrity
- Highest achievable speeds
- 2 DIMM slots total

#### 2T (2 DIMMs per channel)
- More capacity
- More challenging signal integrity
- Lower maximum speeds
- 4 DIMM slots total (standard for most motherboards)

## Signal Groups

### Per Channel Signal Breakdown

#### Data Signals (DQ)
- 64 data bits per channel
- Organized as 8 bytes (8 bits each)
- Bi-directional signals
- Single-ended SSTL (Stub Series Terminated Logic)

#### Data Strobe (DQS)
- 8 DQS pairs per channel (differential)
- DQS_t (true) and DQS_c (complement)
- Used to sample DQ signals
- Critical for timing

#### Data Mask (DM)
- 8 DM signals per channel (one per byte)
- Optional per JEDEC spec
- Can be grounded if not used

#### Command/Address (CA)
- ~14-15 CA signals per channel
- Includes:
  - Address bits: A0-A13 (14 bits)
  - Bank address: BA0-BA1 (2 bits)
  - Bank group: BG0-BG1 (2 bits)
  - ACT_n (activate)
  - RAS_n/CAS_n functionality encoded in CA
- Unidirectional (CPU to DIMM)

#### Clock (CK)
- 1 differential clock pair per channel
- CK_t and CK_c
- Command/address signals referenced to CK
- Data signals referenced to DQS (source synchronous)

#### Control Signals
- CS_n (Chip Select): 2 per channel (one per DIMM)
- CKE (Clock Enable): 2 per channel
- ODT (On-Die Termination): 2 per channel
- RESET_n: Shared or per channel

### Total Pin Count per Channel
- DQ: 64
- DQS: 16 (8 pairs × 2)
- DM: 8
- CA: ~15
- CK: 2 (1 pair)
- Control: ~6-8
- **Total: ~111-113 signals per channel**
- **Both channels: ~222-226 signals**

## Electrical Specifications

### Voltage Levels
- VDD: 1.1V nominal (power supply)
- VDDQ: 1.1V (I/O power)
- VPP: 1.8V (wordline voltage, provided by DIMM PMIC)
- VTT: VDD/2 = 0.55V (termination voltage)
- VREF: VDD/2 = 0.55V (reference voltage)

### Signal Characteristics

#### SSTL (Stub Series Terminated Logic)
- Single-ended signaling for DQ, DM, CA
- Logic levels centered around VREF (0.55V)
- VOH (high): VREF + 0.2V = 0.75V
- VOL (low): VREF - 0.2V = 0.35V

#### Differential Signaling
- Used for DQS and CK
- Differential impedance: 80-100 ohms (typically 80Ω)
- Better noise immunity than single-ended

### Impedance Requirements

#### Single-Ended Signals (DQ, DM, CA)
- Output impedance: 40 ohms (typical)
- Termination: 40 ohms to VTT (on-die or external)
- Total loop impedance: 40 + 40 = 80 ohms

#### Differential Pairs (DQS, CK)
- Differential impedance: 80 ohms
- Each trace: 40 ohms single-ended
- Tight coupling required

## PCB Design Requirements

### Layer Stackup
- Minimum 6 layers for memory routing
- Recommended: 8-10+ layers for full motherboard
- Memory signals on inner layers preferred (better SI)

#### Example Stackup for Memory Area
1. Top: Components, some routing
2. Ground plane
3. DDR5 signals (primary routing layer)
4. Power plane (VDD, VDDQ)
5. DDR5 signals (secondary routing layer)
6. Ground plane
7. Additional signals/power
8. Bottom: Components, routing

### Trace Width and Spacing

#### For 40-ohm single-ended on typical FR-4
- Trace width: ~0.127mm (5mil) typical
- Depends on dielectric height and Er
- Use impedance calculator for exact values

#### For 80-ohm differential
- Trace width: ~0.127mm (5mil) each
- Spacing: ~0.127mm (5mil) between pair
- Keep tight coupling

#### Clearances
- DQ/DQS to other signals: 3× trace width minimum
- Between byte lanes: Can be closer
- To ground: Via stitching for shielding

### Length Matching Requirements

Critical for DDR5 due to high speeds!

#### Within Byte Lane (DQ to DQS)
- Tolerance: ±0.2mm (±8 mils)
- Very tight! Requires careful serpentine routing
- All 8 DQ bits + DM must match to DQS pair

#### DQS to DQS (across byte lanes)
- Tolerance: ±1.0mm (±40 mils)
- Ensures all bytes arrive together

#### DQS to CK
- Tolerance: ±1.0mm (±40 mils)
- Write operations synchronized to CK

#### CA to CK
- Tolerance: ±0.5mm (±20 mils)
- Commands synchronized to CK

#### CK Routing
- Both CK_t and CK_c must be matched
- Equal length to all DIMMs if using 2T topology

### Via Guidelines
- Minimize vias on signal traces
- If vias required, use back-drilling or buried vias
- Via stub < 0.3mm (10 mils) maximum
- Via transition adds ~0.5mm equivalent length

### Routing Topology

#### Point-to-Point (1T)
- CPU → DIMM slot direct
- Cleanest signal path
- Best for high speeds
- Only 1 DIMM per channel

#### Daisy Chain (2T)
- CPU → DIMM1 → DIMM2
- Reflections from stubs
- More challenging to tune
- Required for 2 DIMMs per channel
- Typically limits maximum speed

#### Fly-by Topology
- Used for CA/CK signals in 2T
- CPU → DIMM1 → DIMM2 (continue chain)
- Different delays to each DIMM
- Compensated by memory controller training

### Signal Integrity Considerations

1. **Reference Planes**
   - Continuous ground plane under signals
   - No splits or gaps under traces
   - Via stitching (every 3-5mm along traces)

2. **Crosstalk**
   - 3W spacing rule (3× trace width)
   - DQS pairs isolated from other signals
   - Ground guard traces if needed

3. **Return Path**
   - Keep signal and return paths close
   - Minimize loop area
   - Via stitching across plane transitions

4. **Termination**
   - On-die termination (ODT) in CPU and DIMMs
   - VTT = VDD/2 provided by VRM
   - External termination rarely needed

## DIMM Slot Considerations

### Connector
- DDR5 DIMM: 288-pin connector (UDIMM)
- Notch position prevents DDR4 insertion
- Gold-plated contacts

### Mechanical
- Retention clips at ends
- Proper keepout for DIMM clearance
- Height clearance for tall DIMMs with heatsinks

### Power Delivery to DIMMs
- VDD (1.1V) from motherboard VRM
- VDDQ (1.1V) from motherboard VRM
- PMIC on DIMM generates VPP (1.8V)
- Power pins distributed throughout connector

### SPD and Thermal Sensor
- Each DIMM has SPD chip (Serial Presence Detect)
- I2C/SMBus interface
- Thermal sensor on DIMM (optional)
- Separate I2C bus per DIMM

## Memory Controller Configuration

### CPU Side
- Integrated memory controller in CPU
- Training algorithms:
  - Write leveling
  - Read training
  - Command training
- Adjusts delays to compensate for trace lengths
- BIOS/UEFI handles training during boot

### BIOS/UEFI Settings
- Memory frequency selection
- Voltage adjustments
- Timing parameters
- Training parameters
- XMP/EXPO profile loading

## Decoupling and Power Delivery

### VDD and VDDQ Rails
- Target: 1.1V ±3%
- Current: ~10-20A per channel typical
- Can spike during bursts

### Decoupling Capacitors per Channel
- Bulk: 4-8× 22µF 6.3V (X7R ceramic)
- Medium: 8-16× 10µF 6.3V (X7R ceramic)
- High-freq: 16-32× 1µF 6.3V (X7R ceramic)
- Very high-freq: 32-64× 0.1µF 10V (X7R ceramic)
- Placement: Close to DIMM slots and CPU socket

### VTT Rail (Termination Voltage)
- VTT = VDD/2 = 0.55V
- Can be generated from VDD using voltage divider
- Or dedicated VTT regulator
- Must be stable and low-noise

### VREF (Reference Voltage)
- Per byte lane (8 per channel)
- Generated by resistor divider from VDD
- Very low noise requirement
- Filtered with small capacitor

## Design Validation

### Pre-Layout Simulation
- Signal integrity simulation (HyperLynx, ADS, etc.)
- Check for reflections, crosstalk
- Verify impedance matching
- Optimize topology

### Post-Layout Verification
- Extract parasitics
- Re-simulate with actual layout
- Check for violations
- Iterate if needed

### Hardware Validation
1. **Memory Training Logs**
   - Check BIOS reports
   - Verify all slots train properly
   - Check for margin issues

2. **Memory Testing**
   - MemTest86
   - Prime95 with large FFTs
   - Extended run time (24+ hours)
   - Test at various temperatures

3. **Eye Diagram Measurement**
   - Oscilloscope with high bandwidth
   - Probe DQ/DQS signals
   - Verify eye opening adequate
   - Check for jitter

4. **Performance Testing**
   - Benchmark memory bandwidth
   - Compare to specifications
   - Test at different frequencies

## Common Issues and Solutions

### Memory Won't Train / Boot Fails
- Check power supply to DIMMs
- Verify VTT and VREF voltages
- Check for shorts or opens
- Review routing for length matching violations
- Try single DIMM in each slot

### Instability Under Load
- Insufficient decoupling
- Thermal issues (DIMMs or CPU)
- Marginal signal integrity
- Power delivery issues

### Limited Speed / Won't Overclock
- Poor signal integrity
- 2T topology limitations
- Thermal limitations
- DIMM quality/compatibility

### Intermittent Errors
- Crosstalk from nearby signals
- Power supply noise
- Thermal cycling issues
- Marginal routing

## Design Checklist

- [ ] Channel topology selected (1T or 2T)
- [ ] Impedance targets calculated for stackup
- [ ] Length matching requirements documented
- [ ] DIMM connectors placed with proper spacing
- [ ] Power delivery designed (VDD, VDDQ, VTT, VREF)
- [ ] Decoupling capacitors placed near slots
- [ ] Routing completed with length matching
- [ ] Via count minimized on signal paths
- [ ] Ground planes continuous under signals
- [ ] Via stitching along signal paths
- [ ] SI simulation completed and passing
- [ ] Manufacturing design rules met
- [ ] Test points for key signals (optional)
- [ ] SMBus routing to SPD chips

## Component Count Estimate (Per Channel)

- DIMM slots: 2
- Decoupling caps: ~150-200
- Termination resistors: ~20-30 (if external)
- VREF resistors/caps: ~20
- SMBus pull-ups: 4
- Test points: ~10 (optional)

## Reference Documents

1. JEDEC DDR5 Specification (JESD79-5)
2. Intel Memory Design Guide for LGA1700
3. DIMM connector datasheet
4. Signal integrity simulation guides
5. PCB stackup design guides
6. Memory vendor compatibility lists

## Recommended Tools

- Impedance calculator (e.g., Saturn PCB Toolkit, online calculators)
- Length matching tools in PCB CAD software
- Signal integrity simulator (HyperLynx, ADS, etc.)
- Oscilloscope (for validation)
- Memory testing software

## Cost Estimate (Memory Interface Components)

- DIMM connectors (4×): $8-20
- Decoupling capacitors (600+): $50-100
- Resistors: $5-10
- Testing: Variable

**Total memory interface cost: $65-130**
