[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Using the PiL

**Health Warning!** The Processor-in-the-Loop (PiL) is currently being developed, so there will be frequent updates to the toolbox.

## Overall structure and approach
- **Like the Sandbox, but for real:** The `Pil.slx` simulation extends the capability of the Sandbox with features that allow it to run on a Raspberry Pi. All the tips for running the Sandbox also apply to the PiL. 

- **A virtual tank test:** The PiL has been designed to give you the experience of running a controller remotely in the wave tank. The PiL is the Sandbox with the following additional features:
a configuration parameter set for running on a Raspberry Pi, communications with an Internet-of-Things client (Flespi) to give quasi-real-time interactions with a GUI, power calculations, and a state machine.    
- **Runs:** Just as in a wave tank, the desired wave elevation is synthesised using a DFT, so it is a repeating time series. A 'run' counts as a complete loop through the repeating cycle of waves. The outcome of a run is the average power.       
- **Options for running on a Pi:** The PiL tools can be run on your own Pi. You can also sign up to have time on one of our Raspberry Pis and run this remotely using MATLAB online. 
- **Simulation options:** The PiL can still be run as a Simulation, and there are two options for this: each run can be timed to start at the exact point in the wave sequence, or they can start at different points in the sequence. The first option results in high repeatability, useful for making tweaks to a controller. The second option results in worse repeatability, and this is useful for learning how to adapt to the variability inherent in tank testing. When running on the PiL or in the tank, the natural workflow is to manually start the run using a push button. This would result in a different starting point in the wave sequence. It is useful to have simulations to replicate this.   

## State machine:
The PiL's state machine steps through a sequence of steps that are similar to what would be experienced in the wave tank. The duration of each state can be set in `Init_PiL.m`.

- **SimSetUp:**  The first state gives the hydrodynamic simulation some time to settle. In a wave tank this is analogous to the time the waves take to reach and activate the buoy. For the linear model, this should be at least 20 s. The non-linear models need longer set up times.
- **Ready2Control:** A waiting state. In the wave tank this allows the user to  check whether conditions are right for them before pressing the start button. Alternatively, starting could be automated as soon as this state is reached.
- **ControlRamp:** A ramp from 0 to 1 is multiplied by the demand control force (`F_pto`). We recommend that this is at least ten times longer than the wave period to avoid an offset. 
- **ControlSetUp:** A user-specified time, where the `F_pto` is applied to the hydrodynamic simulation, but the power doesn't contribute to the measurement of average power. This could be useful for bespoke control set up, e.g. allowing a filter to settle.
- **Run:** The period of time used for measurement of the average power. This should always be the repeat time of the DFT. 
- **RampDown:** To prevent a sudden release of PTO force, this should be ramped down. The duration is user-specified; we do not believe that it is critical for this to be too long. 



## Things you're encouraged to change
(All the points mentioned in the Sandbox hold - only variables specific to the PiL are mentioned here)
- **DurationFor:** These are the lengths of time of each of the states. They can be very useful for testing how they impact repeatability.
- **SIMULATION SETTINGS:** The value of `Simulated` controls a switch that sets the `ControlEnabled` variable. The ControlEnabled switch from the GUI is enabled by setting `Simulated = 0`. The non-repeatable runs are chosen using  `Simulated = 1`. The repeatable runs are chosen using  `Simulated = 2`. You can also specify the number of runs done in the simulation (`NumRuns`). When `PiL.slx` is deployed to a Pi then it runs until the GUI stop button is pressed. 


## Things you're discouraged from changing
- **Rasberry Pi settings:** (towards the end of `Init_Sandbox.m') Eventually this will be hidden in the toolbox.
- **The name of the simulation `PiL.slx`:** This will break one of the GUI blocks.
 - **Any code not marked `changeable':** Get in touch with us if you'd like something to be more tweakable.





 




[Previous page (Using the Sandbox)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/2-Using-the-Sandbox.md)

[Next page (Toolbox Updates)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/4-Toolbox-updates.md)
