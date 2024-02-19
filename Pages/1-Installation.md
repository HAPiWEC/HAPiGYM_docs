[Back to documentation hope-page](https://github.com/HAPiWEC/HAPiGYM_docs/blob/main/README.md)

# Getting started - Installation

## 1. Toolbox installation

The software is set up as a Matlab Toolbox and can be installed using the `HAPiGYM.mltbx` file provided to you (your team) at the start of the HAPiGYM process.

**To install:** double click the `HAPiGYM.mltbx` file anywhere inside your operating system's file explorer, or from within Matlab itself.

## 2. Access our cloud-based models

Please visit https://energymodels.eng.ed.ac.uk/accounts/register/ to register for our cloud based model viewer. You'll need the access token provided to you via your welcome email. Once you've registered you can go directly to the step 3. 

## 3. Link your Matlab to our cloud

Please visit https://energymodels.eng.ed.ac.uk/oceanedge/link_matlab to find your API keys. Copy the two lines of code and paste them into your Matlab command line (this will also work on Matlab Online). You can now run your first piece of code!

> **Note:** Running these two lines of code should be a 'one-time' process. Matlab may, however, have to reboot in order to save these environment variables for the next time you `clear` your session. Windows users may also find they need to reboot their OS to have the environment variables persist. In any case, you can re-run the two `setenv(...)` lines as required to let you access our cloud models.

## Updates

This toolbox is maintained by the HAPiWEC Team - updates will be sent to you via email as soon as they are released. You can double click this new file to install the latest version.