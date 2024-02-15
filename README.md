# HAPiGYM Documentation

Welcome to the documentation for the HAPiGYM Software Suite! The tools described in the following form the complete toolset for developing, testing and deploying Wave Energy Converter (WEC) controllers as part of the HAPiGYM experiments. The HAPiGYM concept has been developed as part of a wider project called **HAPiWEC** - please visit https://www.hapiwec.net/ ([EP/V040987/1](https://gow.epsrc.ukri.org/NGBOViewGrant.aspx?GrantRef=EP/V040987/1)) for more information on the project. Details of the HAPiGYM experiments can be found [here](https://www.hapiwec.net/hapigym/).

The software tools have been developed in Matlab and Simulink. For the remote-access stage of the HAPiGYM experiments (i.e. when your controller design is tested remotely in our wave tank facility), interaction with the code will be via Matlab Online.

If you have questions relating to this suite of software tools, please email us (hapiwec@ed.ac.uk) with the subject "HAPiGYM Software Support", or alternatively use the [Contact Us](https://www.hapiwec.net/get-in-touch/) form on our website.


[toc]

---

# Getting started

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