[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Using the Sandbox

**Health Warning!** The sandbox is currently being developed, so there will be frequent updates to the toolbox.

## Overall structure and approach
- **A Raspberry Pi Simulation:** the sandbox is a simulation of code running on a Raspberry Pi. When you are ready to test your code on a Raspberry Pi, you'll be running a simulation called `Pil.slx`. But the first step is to demonstrate that your code is suitable for running on the Pi, and that is what the Sandbox is for. This is important because many of the standard Simulink blocks are continous and won't run on a Pi.
- **Discretisation:** You'll need to replace continuous blocks with their discrete equivalents. To see how this looks for a hydrodynamic model, click on `CPU1>OceanEd_linear`. You'll see that the integrators and the state space model (for the radiation memory) are discrete versions. We had started with a continous model and we discretised it using the [command line](https://uk.mathworks.com/help/simulink/slref/sldiscmdl.html). More information is available [online](https://uk.mathworks.com/help/control/ug/continuous-discrete-conversion-methods.html)       
- **Project usage:** We have assumed that users will make changes to their controllers or to the settings in the initialisation then compare the outcomes of various runs. We anticipate that typically users will only create a new project if there are major changes in their code or in the toolbox structure. (Currently you need admin rights to delete some of the folders created by the hapigym build - we are working on this) However, if there's a major structural change due to a toolbox update, it is possible to revert to an older version of a toolbox[available here](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions/Earlier%20versions). It may be necessary to have a range of project folders to deal with teething problems. 
- **Variant subsystems:** We are using Simulink's variant subsystems to allow users to make subsystem choices. This has an advantage over a switch in that only the chosen variant is compiled. It also makes it easier for different variants to have differently sized inputs and outputs. You can choose which variant to use in the inialisation routine.
- **Library blocks:** Many common components, including the variant subsystems, and your own controller, are now custom Simulink library blocks. Library blocks are a convenient way of making sure that updates are rolled out across muliple simulations, e.g. both the Sandbox and PiL. And unlike referenced models, they are compatible with running on the Pi. These blocks are separate slx files which are stored in the subfolders within `Simulation_components`. They are not yet available from the Library Browser. To insert them in your model, open the library block, copy the contents to the clipboard, and paste into your model. 
- **Modularity:** This use of library blocks and variant subsystems results in a modular approach. It is hoped that this will make the frequent updates less painful for users. 

## Things you're encouraged to change
(Look out for the comment ` % changeable` after each line of code)
- **waves:** In the inialisation routine, you can choose the types of waves. We hope to add to the limited options we are currently offering.
- **PTO SIMULATION SETTINGS:** Currently this selects the variant of PTO efficiency matrix used to calculate electrical power. We will soon add more detailed PTO models as options here, as well as different power ratings. This can be changed by setting the value of `ptoEff_variant` in the PROJECT SETTINGS.
- **CONTROLLER SETTINGS:** This allows users to select the variant of controller used. There is a choice of three dummy controllers offered currently. We will very soon be offering some more interesting options. This can be changed by setting the value of `control_variant` in the PROJECT SETTINGS.
- **POWER CHAIN SETTINGS:** The motor we are using to simulate a generator has an instantaneous torque limit of 49 Nm and a continuous torque limit of 13 Nm. You are welcome to explore how the torque limit impacts your controller, but be aware that you will not have control of this limit during tank testing. You might also be interested in how simulated sensor noise impacts your controller.
- **MyControl:** If we choose the default ```matlab
Variant.Control = 1
``` then we have selected `Simulation_components\Control\MyControlPolicy.slx`. This is the custom library block that users are encouraged to drop their controllers into. If you have different input requirements please let us know. We recommend opening the existing library block, unlocking it, deleting the contents, and pasting in your controller. Once this is saved, the MyController library block in the simulation will update. 

## Things you're discouraged from changing
- **Generate variant control expressions:** (towards the end of `Init_Sandbox.m') Eventually this will be hidden in the toolbox.
- **Subfolder structure and names:** Referenced library blocks will no longer be reachable. 
- **Code in the toolbox:** Horrible horrible things will happen.
- 


## Things you'll be able to change in the future in the future 
- **HYDRO MODEL SETTINGS:** We are working on a non-linear version and a 6 DoF version. Also the good people at NREL are working on a WECSim version. At the moment there is just one option available: a 1 DoF linear model. This is specified as `hydro_model         = "COERbuoy_1DoF_linear_rad400""` in the PROJECT SETTINGS.
- **PROJECT SETTINGS:** There are several parameters in the PROJECT SETTINGS that we will allow users to change in the future, such as `dt`, but this will require testing and documentation first.
- **Raspberry Pi settings:** relevant when things start running on the Pi.
-  **POWER CHAIN SETTINGS:** The parameter `r` is the radius of the shaft that the buoy tether is wound around. For the moment the noise settings are unrealistically dependant on this, so you are currently discouraged from changing this.
- **Variant subsystems:** Perhaps you'd like something we aren't offering? We can add more dummy files if that's useful. If you have anything you are willing to share, we would consider including this as an option. 

 




[Previous page (Quick Start)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/1-Quick-start.md)

[Next page (Toolbox Updates)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Toolbox-updates.md)
