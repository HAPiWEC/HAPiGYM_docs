[Back to documentation home-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Updating the toolbox

### How to upgrade to the latest toolbox
To upgrade to the latest toolbox (which we always recommend), do the following:
1. download the `.mltbx` file from [GitHub](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions) or [OceanEdGE](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions)
2. double-click the file directly from your browser's download list, from you downloads folder, or from within Matlab

**As of HAPiGYM v0.3.3, this is all you need to do!** It is beneficial to understand more about the upgrade process, especially if you have issues working with new versions. Read on to find out more. If you have issues with new upgrades, [older toolbox version can be found here](https://github.com/HAPiWEC/HAPiGYM_docs/tree/main/Toolbox_versions/Earlier%20versions) (for versions earlier than v0.3.3, you will have to type `Hapigym.upgrade()` to synchronise your project with the toolbox version you have chosen).

### What happens when you double-click the `.mltbx` file?
The classes and methods used throughout the HAPiGYM toolbox will all be updated as soon as you double click the `.mltbx` file. Your project(s), however, will still contain the template files that were used when each project was first built, or most recently upgraded. You can see when any of your project(s) were last upgraded by typing `load('meta.mat'); meta.builds`. 

### The automatic upgrade process (happens when you next run your model)
Upgrades to your projects happen automatically when you run an existing model (this new automated feature was added in v0.3.3; users previously had to type `Hapigym.upgrade()` to carry forward the latest files from the add-ons folder, to your local project).

### Which files are overwritten* during automatic upgrading?
The automated upgrade process will check most (but not all) of the files within your project and will compare them to the latest version. The `meta.mat` file contains a list of *upgradable files* which the upgrader uses to identify files to be overwritten* in your project. When an *upgradable file* is out of date, it will automatically be replaced with the latest version. 

### Which files are *not* overwritten during automatic upgrading?
Files which are not listed in `meta.upgradable_files` will remain untouched. This important feature exists to ensure that the user has full control over certain files to allow them to create their own bespoke models. Files that are ***not*** overwritten include:
1. `MyInitialisationFunction.m` - this file allows the user to set up their bespoke model. It is called at the end of the main initalisation function, e.g. `Init_Sandbox`. 
2. `MyControlPolicy.slx` - the user can adapt this library file to build their own Simulink controller, and can expand to further controller options by adding further files (the name of the file(s) won't matter - it will not be overwritten unless it is listed in `meta.upgradable_files`).
It is important to note, that *untracked* files such as `MyControlPolicy.slx` can fall behind the latest template files. This means that newly created projects will have contain `MyControlPolicy.slx` files with more up-to-date content. When a project is upgraded from a previous version, there will be, on occasion, times when you have to add new snippets of code to your older `MyControlPolicy.slx` files. For example, we might choose to increase the number of signals passed to the controller, in which case the demux of the input signals would need to be manually increased.

### How to recover files that you used previously
✱ **NOTE –** Files are not actually overwritten – they are moved to specific locations within the project structure to allow user to see/reverse changes made by the upgrader. Following the initial build process, you may have noticed a number of (nested) folders named `Previous_versions` containing time/version-stamped copies of most files. During upgrades, new copies of the latest template files are also saved to these folders. You can choose to ignore these folders/files, however, they may come in handy when you upgrade the toolbox to later versions. 

If you notice unexpected changes to any working files in this directory following upgrades, check the various `Previous_versions` folders for files with the appendix '_archived'. Files with this appendix are files which the user has changed since a previous build/upgrade, and have since been overwritten* by the upgrade process. You can copy renamed archived files back into the working tree for your project, however, be wary that you may be missing new important code/Simulink context.

Matlab provides a 'right-click' option to compare two selected files, which can help clarify incremental changes.

### How do I persist *my* changes in upgradable files?
If you believe the changes that *you* have made to any upgradable files would benefit other users, please get in touch with the package maintainers (hapiwec@ed.ac.uk) about possible upgrades to the maintained package (master branch).

If you are working on adaptations to certain upgradable files, you can force the upgrader to skip automatic replacement of specific files by removing lines from the `meta.upgradable_files` list within your project's `meta.mat` file. It is likely you will have issues in the future if such files are no longer maintained by us.


[Previous page (Using the Sandbox)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/3-Using-the-PiL.md)

[Next page (Making your own controller)](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/Pages/Getting-started/5-Making-your-own-controller.md)
