[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Using the Sandbox

**Health Warning!** The sandbox is currently being developed, so there will be frequent updates to the toolbox.

## Overall structure and approach
- **A Rasberry Pi Simulation:** the sandbox is a simulation of code running on a Rasberry Pi. When we are ready to share the code for the Pi, you'll be clicking `Run on board` on the hardware tab (rather than `Run` on the Simulation tab). The simulation demonstrates that your code is suitable for running on the hardware. This is important because many of the standard Simulink blocks are continous and won't run on a Pi.
- **Discretisation:** You'll need to replace continuous blocks with their discrete equivalents. To see how this looks for a hydrodynamic model, click on `CPU1>OceanEd_linear`. You'll see that the integrators and the state space model (for the radiation memory) are discrete versions. We had started with a continous model and we discretised it using the [command line](https://uk.mathworks.com/help/simulink/slref/sldiscmdl.html). More information is available [online](https://uk.mathworks.com/help/control/ug/continuous-discrete-conversion-methods.html)       
- **Project usage:** We have assumed that users will make changes to their controllers or to the settings in the initialisation then compare the outcomes of various runs. We anticipate that typically users will only create a new project if there are major changes in their code or in the toolbox structure. (Currently you need admin rights to delete some of the folders created by the hapigym build - we are working on this) However, if there's a major structural change due to a toolbox update, it is possible to revert to an older version of a toolbox[available here](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions/Earlier%20versions). It may be necessary to have a range of project folders to deal with teething problems. 
- **Variant models:** We are using Simulink's variant subsystems to allow users to make subsystem choices. This has an advantage over a switch in that only the chosen variant is compiled. It also makes it easier for different variants to have differently sized inputs and outputs. Additionally, we are implementing the variants as external models rather than subsystems. These models are separate slx files which are stored in the subfolders within `Simulation_components`.  
- **Modularity:** This use of variant models results in a modular approach. It is hoped that this will make the frequent updates less painful for users. This is yet to be seen.

## Things you're encouraged to change

- **waves:** In the `Inialisation.m` routine, you can choose the types of waves. We hope to add to the limited options we are currently offering.
- **PTO SIMULATION SETTINGS:** Currently this selects the variant of PTO efficiency matrix used to calculate electrical power. We have big dreams for adding more detailed PTO models as options here, as well as different power ratings. 
- **CONTROLLER SETTINGS:** This allows users to select the variant of controller used. There is a choice of three dummy controllers offered currently. We will very soon be offering some more interesting options.
- **MyControl:** If we choose the default ```matlab
Variant.Control = 1
``` then we have selected `Simulation_components\Control\MyControlPolicy.slx`. This is the simulation that users are encouraged to drop their controllers into. If you have different input requirements please let us know. 

## Things you're discouraged from changing
- **Generate variant control expressions:** (towards the end of `initialisation.m') Eventually this will be hidden in the toolbox.
- **Subfolder structure and names:** Referenced models will no longer be reachable. 
- **Code in the toolbox:** Horrible horrible things will happen.


## Things you'll be able to change in the future in the future 
- **HYDRO MODEL SETTINGS:** We are working on a non-linear version and a 6 DoF version. Also the good people at NREL are working on a WECSim version.
- **PROJECT SETTINGS:** Watch this space
- **DIGITAL HARDWARE SETTINGS:** relevant when things start running on the Pi.
- **Variant subsystems:** Perhaps you'd like something we aren't offering? We can add more dummy files if that's useful. If you have anything you are willing to share, we would consider including this as an option. 

 




[Previous page (Quick Start)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/1-Quick-start.md)

[Next page (Toolbox Updates)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Toolbox-updates.md)
