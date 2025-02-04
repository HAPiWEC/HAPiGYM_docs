[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Setting up for the first time

## Quick Start Guide <sup>*tl;dr*</sup>


<p style="display:inline-block;">
  <img align="left" src="https://github.com/HAPiWEC/HAPiGYM_docs/assets/39997400/877acd49-b004-4072-b542-2d87089664c6" width="220" title="hover text">

### 1. Install the HAPiGYM Toolbox
Download the most recent version of the toolbox [here (GitHub)](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions). The most recent version is for Matlab R2024b. If you have an earlier version of Matlab, try HAPiGYM_v0-3-9.mltbx in the [Earlier versions](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions/Earlier%20versions) folder. You can also find the latest toolbox [on our supporting web platform, OceanEdGE](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions). From anywhere inside your operating system's file explorer, or from within Matlab itself, double click the `HAPiGYM.mltbx` file. This will add the toolbox to your Matlab add-ons. When you receive notifications about updates to the toolbox, just repeat the above process.

### 2. Build a project
Open Matlab and navigate to a suitable directory for carrying out your work. When you run the following 'one-time' setup command in the command line, Matlab will create a new top-level project directory using the name that you provide when prompted:

```matlab
Hapigym.build_model()
```
You should now see a series of new subdirectories and Matlab/Simulink files, all contained within the new project directory.

### 3. Check the model
Open `Sandbox.slx` and in the Simulation tab of Simuink, select `Run`.

### 4. Upgrade
Download and double-click the latest toolbox file, available from [GitHub](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions) or [OceanEdGE](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions). Your existing projects will automatically be upgraded to the latest version when you next run them. More details on upgrades can be found on the [Toolbox Updates page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Toolbox-updates.md).

</p>

## Explanation

### Toolbox
When you install the toolbox, templates and object-oreintated tools are extracted from the `.mltbx` file into a Matlab-managed toolbox folder. It's hidden away on your system, but is automatically added to your Matlab paths when you start a new session (this is a standard Matlab approach). A series of Matlab *classes* are stored in this location; these can be called directly from any new Matlab session. 

The top-level `Hapigym` class contains some key methods required by general users for:
- creating new models: `Hapigym.build_model()`, 
- loading models: `Hapigym()` (the constructor method), and 
- setting up variant subsystems: `Hapigym.config_VSS()`. 

There are a series of additional methods intended for developers. Note that this main *classdef* is referred to as `Hapigym`, whereas the *toolbox* is referred to as `HAPiGYM`. 

The following Matlab classes are also provided within the toolbox: `PTO`, `Digital_hardware`, `Hydro_model`, `Waves`. Users can inspect/run the main entry-point script `Initialisation.m`, to see how and where these classes are used. Help can be found for each class and method using the usual Matlab approach (e.g. `help Hydro_model`).


### Build
When you use the `build_model` method, selected parts of package (i.e. the files hidden away in the Matlab add-ons folder) are copied to the new project strucutre. **You should keep the filenames and folder structure unchanged going forward**; this is because the main simulation references components in these folders.
You'll still have well-defined areas that you can build your own code and Simulink models.

You will notice an additional file called `meta.mat` is generated during the build process, along with a number of (nested) folders named `Previous_versions` containing time/version-stamped copies of most files. These are created to support the **upgrade process**, which is described in the [Toolbox Updates page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Toolbox-updates.md).

### Initialisation
When you run the `Sandbox.slx` simulation, the first thing that happens is that the `Initialisation.m` file is run. This is buried away in Simulink under `Modelling>Model Settings> Model Properties>Callbacks>PreloadFcn`. Simulink looks for this file in the current folder, so it is important that you are in the current folder when running the simulation. The Initialisation routine lets the user choose settings for that run. These are implemented by creating variables in the workspace which are referenced by the simulation. Some of these workspace variables are objects which are created by calling methods that are defined in the toolbox. `Initialisation.m` is populated with defaults so you can use it straight out-of-the-box. 

### Simulation
After `Initialisation.m` has run, the simulation is built. If additional toolboxes are required, these will be specified in the `Diagnostic Viewer` at the bottom of the Simulink pane. If all is well with the build, the simulation is then run. This is faster than real time, so you might miss it! You can make sure it has actually run using `Data Inspector`, which is available on the Simulation tab in Simulink.       


[Next page (Using The Sandbox)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/2-Using-The-Sandbox.md)
