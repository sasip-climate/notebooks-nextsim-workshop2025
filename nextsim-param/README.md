_2025 SASIP Workshop_

Demonstration session:

## How to Implement a New Parameterization in neXtSIM
Tim Spain and Einar Ólason (NERSC, Bergen, Norway)

### What we will do in this demonstration session:
We will examine how to write a new parameterisation for neXtSIM. In the example, we'll write a parameterisation of lateral ice formation and melt based on Mellor and Kantha (1989). We will try it out in the polynya setup from the nextsimdg session. We will focus on ice formation, as that setup does not cover ice melt. The work requires understanding the program structure and design presented earlier in the day.

### The main steps to create a new parameterisation, using the existing module infrastructure:

 0. [Fork](https://github.com/nextsimhub/nextsimdg/fork) the [nextsimdg](https://github.com/nextsimhub/nextsimdg) repository and work on your branch.
 1. Identify the module to which the parameterisation belongs. In the demonstration case, this is the LateralSpreadModule in the [physics/src/modules/LateralIceSpreadModule](https://github.com/nextsimdg/nextsimdg/blob/1279acb6220bfbdd20ec847e5dfcbf992424a072/physics/src/modules/LateralIceSpreadModule]) directory.
 2. Implement the new functionality in a new source file (.cpp) and header file in the include directory. Make sure the new class inherits from the interface base class. In the demonstration case, this is [physics/src/modules/include/ILateralIceSpread.hpp](https://github.com/nextsimdg/nextsimdg/blob/b0ad5cf86e6082d2134c43a9b8ec2b286009cb63/physics/src/modules/include/ILateralIceSpread.hpp).
 3. Add the new module to the module.cfg file. In the demonstration case, this is [physics/src/modules/LateralIceSpreadModule/module.cfg](https://github.com/nextsimdg/nextsimdg/blob/60d09b143af90a1719dcdfb508173891d148bffc/physics/src/modules/LateralIceSpreadModule/module.cfg)
Rerun cmake to auto-generate the module.cpp file in the module directory.
 4. (Optional) Create a pull request on GitHub to ask to merge your new module into the main model repository.

### References:

Mellor, G. L., & Kantha, L. (1989). An ice-ocean coupled model. Journal of Geophysical Research, 94(C8), 10937–10954. [JC094iC08p10937](https://doi.org/10.1029/JC094iC08p10937).
