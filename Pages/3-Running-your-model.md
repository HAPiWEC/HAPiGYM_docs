[Back to documentation hope-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Getting started - Running your model

The first step anytime you open Matlab is to open the `Initialisation.m` file and run/step-through the code. Once you've run this you should find that you have populated the workspace with all the variables need for the Simulink model(s). If you've come straight here from the [Build a model](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/2-Build-a-model.md) stage (i.e. you've just run `Hapigym.build_model()`), the model should run straight out-of-the-box, with default applied of course!

The different Simulink components of your new model are found in the root of your project path (specifically the `Outline.slx` file), and within the `Simulation_components` folder.

The next stage of the HAPiGYM process is to start tailoring this `Initialisation.m` script for your specific needs, and to start developing your own controller.

[Previous page (Build a model)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/2-Build-a-model.md)
[Next page (Making your own controller)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/4-Making-your-own-controller.md)
