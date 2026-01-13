# VRM (Voltage Regulator Module) Power Delivery Design

## Overview
The VRM is responsible for converting the 12V input from the power supply into the various voltage rails required by the CPU and other components. This is one of the most critical and complex subsystems.

## Power Requirements Summary

### CPU Power Rails

1. **VCCIN (Input Voltage)**
   - Input: 12V (from EPS connector)
   - Output: 1.8V nominal (1.5V - 2.0V range)
   - Current: Up to 100A peak
   - Phases: 4-6 phases recommended

2. **VCC (Core Voltage)**
   - Input: 12V (from EPS connector)
   - Output: 0.6V - 1.5V (dynamically controlled)
   - Current: Up to 300A+ peak (varies by CPU model)
   - Phases: 12-20 phases recommended
   - This is the PRIMARY and most demanding rail

3. **VCCSA (System Agent)**
   - Input: 5V or 12V
   - Output: ~1.05V (adjustable for overclocking)
   - Current: Up to 10A
   - Phases: 1-2 phases

4. **VDD (DDR5 Memory I/O)**
   - Input: 5V or 12V
   - Output: 1.1V nominal
   - Current: Up to 20A per channel (40A total)
   - Phases: 2-4 phases

5. **VCC_GT (Graphics - iGPU)**
   - Input: 12V
   - Output: ~1.05V (adjustable)
   - Current: Up to 30A
   - Phases: 2-4 phases

## VRM Topology

### Recommended: Multiphase Buck Converter

#### Why Multiphase?
- Distributes heat across multiple components
- Reduces input/output ripple
- Improves transient response
- Allows smaller inductors and capacitors per phase
- Better efficiency

#### Phase Count Guidelines
- Budget design: 8-10 phases for VCC
- Mainstream: 12-16 phases for VCC
- High-end: 18-20+ phases for VCC
- Extreme OC: 24+ phases (with doublers/triplers)

### Phase Doubler/Tripler
- Can use PWM controller with fewer channels + doublers
- Example: 8-channel PWM controller + doublers = 16 phases
- Reduces cost but may have slightly different characteristics

## VRM Controller Selection

### Popular Controllers

1. **Intersil/Renesas (High-end)**
   - RAA229618 (18-phase)
   - RAA229131 (13-phase)
   - Features: PMBus, telemetry, adaptive voltage positioning

2. **International Rectifier/Infineon**
   - IR35217 (7+1 phase)
   - Features: Digital control, PMBus

3. **uPI Semiconductor**
   - uP9512R (8-phase)
   - uP9521 (11-phase)

4. **ON Semiconductor**
   - NCP81274 (Dual-phase)

### Controller Requirements
- Intel VR13.5 or VR14 compliant
- SVID (Serial VID) interface support
- Load line calibration
- Over-current protection (OCP)
- Over-voltage protection (OVP)
- Thermal monitoring
- Power state management (C-states)

## Power Stage Components

### Driver + MOSFET vs. Integrated Power Stage

#### Option 1: Discrete MOSFETs + Driver
**Advantages:**
- More flexibility
- Can handle higher currents
- Better for extreme designs

**Components per phase:**
- 1x Gate driver IC
- 1-2x High-side MOSFET
- 1-2x Low-side MOSFET
- Supporting passives

**Example MOSFETs:**
- High-side: Infineon BSC0906NS (RDS(on) = 0.9mΩ)
- Low-side: Infineon BSC0909NS (RDS(on) = 0.9mΩ)

#### Option 2: Integrated Power Stage (DrMOS)
**Advantages:**
- Simpler design
- Smaller footprint
- Integrated driver and MOSFETs
- Better thermal performance

**Recommended for this project**

**Example Power Stages:**
- Infineon TDA21490 (90A capable)
- ON Semiconductor NCP303090 (90A capable)
- Vishay SIC632 (60A capable)
- International Rectifier IR3599 (60A capable)

### Current Rating per Phase
- VCC rail example: 300A total / 16 phases = ~19A per phase
- Recommend 50A+ rated power stages for headroom
- Typical: 60A-90A power stages

## Inductor Selection

### Requirements
- Inductance: 0.3µH - 1.0µH typical
- Saturation current: > 50A per phase
- DCR (DC Resistance): < 0.5mΩ
- Shielded type preferred (reduces EMI)

### Recommendations
- 0.47µH for VCC phases
- 1.0µH for VCCIN phases
- Use high-quality inductors (Vishay, Coilcraft, TDK)

### Footprint Considerations
- Typical size: 7mm x 7mm to 10mm x 10mm
- Power inductors generate heat - adequate spacing needed
- Consider airflow direction

## Capacitor Selection

### Input Capacitors (12V rail)
- Bulk capacitance: 1000µF - 2000µF total
- Polymer aluminum or electrolytic
- Low ESR (<20mΩ)
- Voltage rating: 25V minimum (35V preferred)
- Multiple capacitors in parallel
- Placement: Close to EPS connector and power stages

### Output Capacitors (CPU rails)

#### VCC Rail (most critical)
- Bulk capacitance: 3000µF - 10000µF total
- **Polymer capacitors required**
- Low ESR (<2mΩ combined)
- Voltage rating: 6.3V or 10V
- Typical: 50-100 capacitors of various values
  - 470µF x 10-20 (bulk)
  - 220µF x 10-20 (bulk)
  - 100µF x 10-20 (medium)
  - 47µF x 10-20 (medium)
  - 22µF x 10-20 (response)

#### VCCIN Rail
- Bulk capacitance: 500µF - 1000µF total
- Polymer capacitors
- Voltage rating: 6.3V
- Multiple values (100µF, 47µF, 22µF)

#### Other Rails (VCCSA, VDD, VCC_GT)
- 200µF - 500µF per rail
- Combination of polymer and ceramic
- Appropriate voltage ratings

### Ceramic Capacitors (High frequency)
- 10µF x 20-40 (X7R, 10V or higher)
- 1µF x 40-60 (X7R, 10V or higher)
- 0.1µF x 60-100 (X7R, 16V or higher)
- Placement: Distributed around socket, close to power pins

### Capacitor ESR and ESL
- ESR (Equivalent Series Resistance): Lower is better
- ESL (Equivalent Series Inductance): Lower is better
- Parallel combination reduces effective ESR/ESL

## PCB Layout Considerations

### Critical Layout Rules

1. **Power Stage Placement**
   - Arrange in organized rows around socket
   - Equal spacing for thermal balance
   - Good airflow path

2. **Input Power Path**
   - Wide, short traces from EPS connector to input caps
   - 12V input: 2-4mm wide traces minimum
   - Use multiple vias to power planes
   - Minimize impedance

3. **Output Power Path**
   - Wide, short traces from power stages to CPU socket
   - VCC output: 2-4mm wide traces minimum
   - Use power planes where possible
   - Multiple vias to distribute current

4. **Ground Return Path**
   - Solid ground plane under power stages
   - Via stitching around power area
   - Minimize ground loop area

5. **Switching Node**
   - Keep traces short between MOSFET, inductor
   - This is a high dI/dt node (high noise)
   - Keep away from sensitive signals

6. **Gate Drive Signals**
   - Keep traces short from controller to drivers/power stages
   - Controlled impedance if using long traces
   - Avoid crossing over noisy signals

7. **Feedback and Sense Lines**
   - Kelvin connections to output
   - Separate sense traces to VRM controller
   - Keep away from switching nodes
   - Use ground guard traces if needed

### Thermal Management

1. **Copper Thickness**
   - 2oz copper minimum for power planes
   - 3-4oz for extreme designs
   - Inner layers can use 1oz

2. **Heatsinks Required**
   - Power stages generate significant heat
   - Active cooling recommended (heatsink + fan)
   - Thermal pads between power stages and heatsink
   - VRM heatsink typically 100-300g

3. **Thermal Vias**
   - Array of thermal vias under power stages
   - Connect to inner/bottom copper layers
   - Typical: 0.3mm diameter, 0.8mm spacing array

4. **Component Spacing**
   - Adequate spacing between heat-generating components
   - Consider airflow direction
   - Don't trap heat

## Control and Monitoring

### SVID Interface
- Serial VID (Voltage Identification)
- Communication between CPU and VRM controller
- Signals: SVID_CLK, SVID_DATA
- Pull-up resistors required
- Short traces, avoid stubs

### PMBus (optional but recommended)
- I2C-based communication
- Allows monitoring and control of VRM
- Telemetry: voltage, current, temperature, efficiency
- Useful for debugging and optimization

### Enable Signals
- PWR_OK from power supply
- Various enable sequencing
- Proper power-up/power-down sequence critical

### Voltage Sense
- Remote sense connections to socket
- Compensates for voltage drop
- Critical for accuracy

## Component Bill of Materials (BOM) - VRM Section

### VCC Rail (16-phase example)
- 1x VRM Controller (e.g., RAA229618)
- 16x Power Stage (e.g., TDA21490 or DrMOS)
- 16x Inductor 0.47µH 50A+ (e.g., Vishay IHLP-5050)
- 80x Polymer capacitor 470µF 6.3V
- 60x Polymer capacitor 220µF 6.3V
- 40x Ceramic capacitor 10µF X7R 10V
- 60x Ceramic capacitor 1µF X7R 10V
- 80x Ceramic capacitor 0.1µF X7R 16V
- Input caps: 4x 470µF 25V electrolytic/polymer
- Resistors/small signal components: ~50-100

### VCCIN Rail (4-phase example)
- Can use same controller (multi-rail) or separate
- 4x Power Stage
- 4x Inductor 1.0µH
- ~30x Polymer capacitors (various values)
- Supporting ceramics

### Other Rails
- Simpler designs, 1-2 phases each
- Smaller power stages or discrete
- Fewer passives

### Total Component Count (VRM only)
- Estimate: 500-800 components just for power delivery

## Testing and Validation

### Required Tests
1. **Static Load Test**
   - Electronic load to verify regulation
   - Check voltage accuracy
   - Measure ripple

2. **Dynamic Load Test**
   - Fast load transients
   - Verify transient response
   - Check overshoot/undershoot

3. **Efficiency Measurement**
   - Measure input power and output power
   - Target: >85% efficiency at typical load

4. **Thermal Testing**
   - Measure component temperatures
   - Verify heatsink effectiveness
   - Check for hot spots

5. **Ripple and Noise**
   - Oscilloscope measurement
   - Target: <10mV peak-to-peak ripple
   - Check across frequency range

### Debug Features to Include
- Test points for all voltage rails
- Current sense resistors (optional, for monitoring)
- Access to enable/fault signals
- LED indicators for power good

## Common Issues and Troubleshooting

### Instability
- Insufficient output capacitance
- Poor feedback compensation
- Layout issues (excessive inductance)

### Overheating
- Inadequate heatsinking
- Poor thermal via design
- Insufficient copper thickness
- Over-current condition

### High Ripple
- Insufficient capacitance
- Poor capacitor selection (high ESR)
- Layout issues

### Voltage Droop
- Inadequate phase count
- Poor load line calibration
- Excessive resistance in power path

## Advanced Features (Optional)

### Adaptive Voltage Positioning (AVP)
- Intentional load line (droop)
- Reduces voltage at light load
- Improves efficiency

### Power State Management
- C-state support
- P-state voltage scaling
- Reduces idle power

### Overclocking Features
- Adjustable voltage rails
- Load line calibration control
- Current limit adjustment
- Usually controlled via BIOS

## Design Checklist

- [ ] Controller selected and VR13.5/VR14 compliant
- [ ] Phase count adequate for target CPU
- [ ] Power stages rated for required current
- [ ] Inductors selected with proper saturation rating
- [ ] Sufficient output capacitance (especially VCC)
- [ ] Input capacitance adequate
- [ ] PCB has enough copper thickness
- [ ] Thermal management plan (heatsinks)
- [ ] SVID interface properly implemented
- [ ] Power sequencing correct
- [ ] Voltage sensing implemented
- [ ] Protection features included (OCP, OVP, thermal)
- [ ] Test points for all rails
- [ ] Layout follows best practices

## Cost Estimate (VRM Components Only)

- VRM Controllers: $10-30
- Power Stages (20 total): $40-100
- Inductors (20 total): $20-40
- Capacitors (500+ total): $100-200
- VRM Heatsink: $20-50
- Misc (resistors, etc.): $10-20

**Total VRM cost: $200-440**

## References Needed

1. Intel VR13.5/VR14 Design Guide
2. Controller datasheet (RAA229618 or equivalent)
3. Power stage datasheet
4. Intel processor power specifications
5. Application notes on multiphase VRM design
6. PCB layout guidelines for power delivery
