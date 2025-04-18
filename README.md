# V2G & EV Penetration Impact on Manchester LV Network – OpenDSS Simulation

## 2. Introduction

This project investigates the impact of electric vehicle (EV) charging and Vehicle-to-Grid (V2G) integration on the Manchester Low Voltage (LV) distribution network using OpenDSS. The simulation focuses on uncoordinated charging behaviour and evaluates how increasing levels of EV and V2G penetration affect voltage stability, transformer loading, and system losses. The study uses a modified IEEE 13-bus system representing Manchester’s real-world LV distribution topology based on Electricity North West Limited (ENWL) data.

The project explores multiple penetration levels—ranging from 20% to 100%—of EV-only and V2G-enabled households, incorporating realistic household load profiles with 5-minute resolution to reflect winter peak demand conditions.

## 3. Diagram/Image/Demo

You may refer to the following figure for a visual comparison of load behaviour under different scenarios:

**Load Profile at Bus 646 for 100% EV vs 100% V2G Penetration**

![Bus 646 Load Profile](./Picture1.png)

## 4. User Installation Instructions

1. **Install OpenDSS**:
   - Download and install OpenDSS from the official Electric Power Research Institute (EPRI) website: https://www.epri.com/pages/sa/opendss

2. **Directory Setup**:
   - Create a project directory: `EV_V2G_Manchester_Simulation/`
   - Inside this folder, create subfolders for:
     - `CSV_LoadProfiles` → contains all 5-minute resolution `.csv` files for each bus under various penetration scenarios.
     - `IEEE13BusBase` → includes `IEEELineCodes.dss` and `IEEE13Node_BusXY.csv`
     - Place `simulation.dss` (your main file) in the root directory.

3. **Check File Paths**:
   - Make sure to update any absolute paths in the code to match your local machine, especially those pointing to the `.csv` files for LoadShapes.

4. **Dependencies**:
   - No extra Python or MATLAB libraries required unless you're generating the `.csv` LoadShape data externally.

## 5. How to Run the Code

1. Open OpenDSS.
2. Navigate to the folder containing your `simulation.dss`.
3. Run the following command in the OpenDSS Command Window:

```plaintext
Redirect simulation.dss
```

4. **Switching Between Scenarios**:
   - To simulate a specific EV or V2G scenario, **uncomment** the relevant `New Load` definitions (e.g. `Bus646(20)` for 20% EV penetration) and **comment out** the others.
   - Example:
     ```dss
     ! Comment out:
     !New Load.646 Bus1=646... daily=Bus646(0)

     ! Uncomment this to simulate 20% EV penetration:
     New Load.646 Bus1=646... daily=Bus646(20)
     ```

5. Use `Plot` or `Export` commands already included in the file to visualize voltages or system losses.

## 6. More Technical Details

- **Load Profiles**: All profiles are based on 5-minute intervals (288 points per day) under winter conditions, derived from ENWL data representing real household demand in Manchester.
- **Network Topology**: The IEEE 13-bus feeder is modified to reflect the real-world layout of Network 2 (567 households distributed across 5 feeders).
- **Transformers & Voltages**: Distribution transformers were added to step down 11 kV to 0.416 kV. The substation is modeled as a stiff source with 132/11 kV transformation.
- **Power Flow Settings**:
  - Simulation mode: `daily`
  - Time step: `5m`
  - Control mode: `time`
- **Monitors & Exported Results**: Voltage, transformer loading, line losses, and substation energy flows are monitored and exported using OpenDSS’s `Monitor` and `EnergyMeter` features.
- **All parameters such as line impedance, transformer ratings, and load allocation were sourced from ENWL documentation** and aligned with Manchester LV network configurations.

## 7. Known Issues / Future Improvements

- **No Load Scheduling**: Charging and discharging behaviours are uncoordinated. Adding a coordinated schedule or price-based response would improve realism.
- **EV Travel and Arrival Times**: Stochastic modelling of travel behaviour was done externally in MATLAB; integrating it into OpenDSS would streamline simulation.
- **Parameter Scaling**: Current load values are scaled but not normalized for seasonal variations or forecast growth.
- **Regulator Control Disabled**: Voltage regulator tap controls were disabled for simplification. This may need enabling in future studies.
- **Graph Generation**: All plotting is done using OpenDSS. A Python/Matlab script could automate visualisation and batch simulations.
