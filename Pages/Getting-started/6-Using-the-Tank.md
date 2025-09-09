[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Using the Tank

> :warning: **Health Warning!** The Tank simulation (`Tank.slx`) is the most complex and closest-to-reality option, designed for controlling physical hardware. It is under active development and interfaces with live systems; use it with extreme caution.

> **Prerequisite:** Before using the Tank, you should be completely familiar with the [Sandbox](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/2-Using-the-Sandbox.md) and [PiL](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Using-the-PiL.md). The Tank model builds directly upon these.

## Overall structure and approach
- **Like the PiL, but for hardware:** The `Tank.slx` simulation extends the PiL with the critical hardware interfaces and safety protocols required for operating the physical OSPREY rig in the wave tank or the Bobblehead emulator on a lab bench.
- **A real tank test:** The Tank is designed for final pre-tank testing and operation. It replaces simulated hydrodynamics and PTO with live data from sensors (encoders, torque) and sends real force demands to the motor drives.
- **Safety First:** The model includes a comprehensive state machine and a dual-GUI system that manages the crucial handover of control between the **Tank Operator** (ensures safe startup) and the **User** (runs the controller).
- **Experiment Variants:** The model's behaviour is defined by the `experiment_variant` setting:
    - **`1`: `Rig_Sim`**: A pure simulation on your computer (identical to PiL). Use for final logic checks.
    - **`2`: `OSPREY_II`**: For the **real OSPREY rig in the wave tank (FloWave)**. Interfaces with actual hardware.
    - **`3`: `WaveEmulator1`**: A **virtual emulator** for the Bobblehead lab bench setup. Uses a simplified model for HIL testing without complex wave physics.

## State machine:
The Tank's state machine is more detailed than the PiL's, reflecting real-world operational and safety procedures. The duration of each state can be set in `Init_Tank.m`.

- **TankRampUp:** The tank operator applies a pre-load force to the system to establish a safe starting position.
- **Ready2Control / TankManualControl:** Waiting states where the Tank Operator has control, managing the handover to the user.
- **ControlSetUp:** The user's controller is enabled and given time to initialize (e.g., for filter states to settle before the main run).
- **Wait4ZeroPos:** The system waits for the WEC to pass through the equilibrium (zero) position before applying the full control force. This prevents a damaging "jerk" on the hardware.
- **Run:** The main data collection period where the user's controller is active and power is calculated.
- **RampDown:** The control force is smoothly ramped down to zero to avoid sudden force transitions on the hardware.
- **RunDone:** The run is complete. Control is returned to the Tank Operator.

## Things you're encouraged to change
(All the points mentioned in the Sandbox and PiL hold - only variables specific to the Tank are mentioned here)
- **`EXPERIMENT SETTINGS`:** The `experiment_variant` is the most critical setting. Choose the correct variant for your task (`1` for Simulation, `3` for Bobblehead Emulator, `2` for Real Tank).
- **`GUI SETTINGS`:** The `GUI_variant` determines how you interact with the system. Choose between simulated presses (`1`), remote Dual GUI (`2`), or the local Combined GUI dashboard (`3`).
- **`TIMING SETTINGS`:** The `DurationFor` each state (e.g., `Wait4ZeroPos`, `ControllerSetUp`) in `Init_Tank.m` are critical for safe and effective operation and can be tuned.
- **`POWER CHAIN SETTINGS`:** The radius `PTO.r` **must** be set to the exact physical radius of the winch drum on the specific rig you are using (OSPREY vs. Bobblehead have different drums).

## Things you're discouraged from changing
- **The State Machine Logic and Transitions:** The sequence of states is designed for hardware safety. Do not modify the logic or order of states within the Simulink model.
- **Hardware Interface Blocks:** The blocks that read encoders, send torque demands, and handle emergency stops are configured for specific hardware. Changing them is a safety risk and will likely break hardware communication.
- **Safety-Critical Defaults:** Values like the torque limit (`PTO.T_max`) are set to the hardware's real, physical limits. Increasing these values will not make the hardware more powerful and could be dangerous.
- **The name of the simulation `Tank.slx`:** This will break the configuration and GUI blocks designed to work with this specific model name.
- **Any code not marked `changeable`:** Get in touch with us if you'd like something to be more tweakable.

## Workflow for a Tank Test

- **Develop** your controller in the `Sandbox.slx`.
- **Test** its behaviour on a Raspberry Pi using `PiL.slx` in deployed mode.
- **Verify** the full run procedure and timing using `Tank.slx` with `experiment_variant = 1` (Rig_Sim).
- **(Optional)** Perform emulator testing with `Tank.slx` using `experiment_variant = 3` (WaveEmulator1) on the Bobblehead lab bench setup.
- For a **real tank test**, a trained operator will run `Tank.slx` with `experiment_variant = 2` (OSPREY_II). You will then use your GUI to enable control and start your run when instructed.


[Previous page (Making your own controller)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/5-Making-your-own-controller.md)
