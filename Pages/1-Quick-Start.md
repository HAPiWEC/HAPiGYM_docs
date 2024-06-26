[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Setting up for the first time


## Quick Start Guide


<p style="display:inline-block;">
  <img align="left" src="https://github.com/HAPiWEC/HAPiGYM_docs/assets/39997400/877acd49-b004-4072-b542-2d87089664c6" width="220" title="hover text">
 
**1. Install the HAPiGYM Toolbox:** Download the most recent version of the toolbox [here](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions). From anywhere inside your operating system's file explorer, or from within Matlab itself, double click the `HAPiGYM.mltbx` file. This will add the toolbox to your Matlab add-ons.

**2. Build a project:** Create an empty directory to carry out your work. Open Matlab and move to this path, then run the following 'one-time' setup command in the command line:

```matlab
Hapigym.build_model()
```
**3. Check the model:** Open `Outline.slx` and in the Simulation tab of Simuink, select `Run`.

</p>

## Explanation

**Toolbox:** When you install the toolbox, templates and object-oreintated tools are downloaded into a toolbox folder. It's hidden away on your system, but on your Matlab path. 

**Build:** When you use the method `build_model` an instance of the project strucutre is copied over from the toolbox folder to the folder you are in when you do the build. You'll now find a folder structure in the present directory. **You should keep the filenames and folder structure unchanged going forward**; this is because the main simulation references components in these folders.
You'll still have well-defined areas that you can build your own code and Simulink models

 **Initialisation:** When you run the `Outline.slx` simulation, the first thing that happens is that the `Initialisation.m` file is run. This is buried away in Simulink under `Modelling>Model Settings> Model Properties>Callbacks>PreloadFcn`. Simulink looks for this file in the current folder, so it is important that you are in the current folder when running the simulation. The Initialisation routine lets the user choose settings for that run. These are implemented by creating variables in the workspace which are referenced by the simulation. Some of these workspace variables are objects which are created by calling methods that are defined in the toolbox. `Initialisation.m` is populated with defaults so you can use it straight out-of-the-box. 
 
 **Simulation:** After `Initialisation.m` has run, the simulation is built. If additional toolboxes are required, these will be specified in the `Diagnostic Viewer` at the bottom of the Simulink pane. If all is well with the build, the simulation is then run. This is faster than real time, so you might miss it! You can make sure it has actually run using `Data Inspector`, which is available on the Simulation tab in Simulink.       


[Next page (Using The Sandbox)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/2.Using-The-Sandbox.md)
