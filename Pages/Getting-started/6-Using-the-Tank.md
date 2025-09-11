[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Using the Tank

> :warning: **Health Warning!** The Tank simulation (`Tank.slx`) is the most complex and closest-to-reality option, designed for controlling physical hardware. It is under active development and interfaces with live systems; use it with extreme caution.

> **Prerequisite:** Before using the Tank, you should be completely familiar with the [Sandbox](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/2-Using-the-Sandbox.md) and [PiL](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Using-the-PiL.md). The Tank model builds directly upon these.

## Overall structure and approach
- **The Bridge to Hardware:** The `Tank.slx` simulation extends the PiL with the critical hardware interfaces and safety protocols required for operating the physical OSPREY rig in the wave tank. It can also run in a pure simulation mode (`Rig_Sim`).
- **Safety First:** The model includes a comprehensive state machine and a multi-GUI system that manages the crucial handover of control between the **Tank Operator** (ensures safe startup) and the **User** (runs the controller).
- **Variant Combinations:** The following table shows which combinations of Experiment and GUI variants are used together:

|                            |    **Rig_Sim**   | **OSPREY_II** |
|----------------------------|:---------------:|:---------------:|
| **SimulatedButtonPresses** |  Simulation>Run |        x        |
| **DualGUI**                | Hardware>Deploy | Hardware>Deploy |
| **CombinedGUI**            |        x        | Hardware>Monitor |

## State machine:
The Tank's state machine is more detailed than the PiL's, reflecting real-world operational and safety procedures. The duration of each state can be set in `Init_Tank.m`. The table below details each state's purpose and how it is triggered for different GUI variants:

| **State**             | **#** | **Purpose**                                                                       | **SimulatedButtonPresses**                                                                         | **Dual/Combined GUI (TankTesting)**                                               |
|-----------------------|:-----:|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| **TankManualControl** |  10   | Tank operator applies preload and starts waves                                    | Timeseries _SimulatedTankControl_ in work space, generated using _DurationFor.SimulateTankControl_ | Tank operator toggles a switch on the GUI when it is safe for user to have control. |
| **TankRampUp**        |   1   | Time for waves to build up                                                        | Timer with _DurationFor.BuoyToReachEquilbrium_                                                     | Timer with _DurationFor.BuoyToReachEquilbrium_                                    |
| **Ready2Control**     |   2   | Monitor position in waves prior to zeroing                                        | Timeseries SyncRunStart in work space, generated using _DurationFor.BuoyToReachEquilbrium_         | The _Controller ON_ button is toggled on the GUI                                  |
| **ControlSetUp**      |   4   | Position is zeroed but control not connected; time for filters to stabilize       | Timer with _DurationFor.ControllerSetUp_                                                           | Timer with _DurationFor.ControllerSetUp_                                          |
| **Wait4ZeroPos**      |   5   | Full use in future controllers; presently starts controller                       | Timer with _DurationFor.Wait4ZeroPos_                                                              | Timer with _DurationFor.Wait4ZeroPos_                                             |
| **Run**               |   6   | Timed collection of data to calculate average power                               | Timer with _DurationFor.Run_; the repeat time of simulated wave (e.g., 62.83s).                    | Timer with _DurationFor.Run_; **repeat time of wave chosen in the tank.**         |
| **RampDown**          |   7   | Controller signal is ramped down                                                  | Timer with _DurationFor.RampDown_                                                                  | Timer with _DurationFor.RampDown_                                                 |
| **RunDone**           |   8   | Signals for experiment to end or an additional run to start (state 2)             | Simulation length (workspace variable) determined by chosen value of _NumRuns_.                    | Status bars on dual GUI prompt user to stop simulation, or start another run.     |

## Things you're encouraged to change
(All the points mentioned in the [Sandbox](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/2-Using-the-Sandbox.md) and [PiL](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Using-the-PiL.md) hold - only variables specific to the Tank are mentioned here)
- **`PTO_variant`:** Choose between efficiency matrices for power calculation (`1` for optimised, `2` for based on a real PTO).
- **`PTO_settings`:** Parameters in the `PTO = PowerTakeOff(...)` definition (e.g., shaft radius, maximum torque, noise levels for sensors).
- **`Controller`:** Choose between four controller options in `control_variant`:
    - `1`: **MyControlPolicy** (User-designed controller in `MyControlPolicy.slx`)
    - `2`: **ControlConstantSpringDamping**
    - `3`: **DummyControlZeroForce** (Useful for commissioning)
    - `4`: **Adaptive Linear Damping (ALD)** (Note: configured for CombinedGUI; sign conventions with DualGUI need testing)
- **`GUI SETTINGS`:** The `GUI_variant` determines how you interact with the system. `1` for simulating button presses, `2` for operation on Pi via FlesPi (Dual GUIs), `3` for running on OSPREY (Combined GUI).
- **`EXPERIMENT SETTINGS`:** The `experiment_variant` is the most critical setting. Choose `1` for Simulation/Rig Testing, `2` for the Real Wave Tank (OSPREY_II).
- **`TIMING SETTINGS`:** The `DurationFor` each state in `Init_Tank.m` are critical for safe and effective operation and can be tuned. **Note:** `DurationFor.Run` should be set to the repeat period of the wave being used in the tank.

## Things you're discouraged from changing
- **The State Machine Logic and Transitions:** The sequence of states is designed for hardware safety. Do not modify the logic or order of states within the Simulink model.
- **Hardware Interface Blocks:** The blocks that read encoders, send torque demands, and handle emergency stops are configured for specific hardware. Changing them is a safety risk and will likely break hardware communication.
- **Safety-Critical Defaults:** Values like the torque limit (`PTO.T_max = 12`) and shaft radius (`PTO.r = 0.03`) are set to the hardware's real, physical limits. **Do not change these for the OSPREY_II variant.**
- **The name of the simulation `Tank.slx`:** This will break the configuration and GUI blocks designed to work with this specific model name.

## Workflow for a Tank Test

- **Develop** your controller in the `Sandbox.slx`.
- **Test** its behaviour on a Raspberry Pi using `PiL.slx` from the Hardware tab in deployed mode.
- **Verify** the full run procedure and timing using `Tank.slx` with `experiment_variant = 1` (Rig_Sim) and `GUI_variant = 1`. Run via the Simulation tab.
- For a **real tank test**, users have two options:
    1.  **Build & Deploy** their controller in `Tank.slx` on their own using MATLAB Online. Then use the **User GUI** to enable control and monitor experiment data. This uses the **Dual GUI** (`GUI_variant = 2`).
        (MATLAB Online option is currently unavailable, details can be seen in https://ww2.mathworks.cn/matlabcentral/answers/2178977-matlab-online-2025a-not-connecting-with-raspberry-pi)
    3.  Share their controller with the **Tank Operator**. The operator will then **Monitor & Tune** the user's `Tank.slx` at the tank site. Data can be monitored using the **Data Inspector**. This uses the **Combined GUI** (`GUI_variant = 3`).

[Previous page (Making your own controller)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/5-Making-your-own-controller.md)
