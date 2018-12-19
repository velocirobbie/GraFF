# GraFF
## A Graphene and Graphite Forcefield for LAMMPS

This is a guide to using the GraFF forcefield for graphitic carbons in LAMMPS.
Full details of the forcefield can be found in the paper:

Sinclair, Robert C., James L. Suter, and Peter V. Coveney. "Graphene–Graphene Interactions: Friction, Superlubricity, and Exfoliation." Advanced Materials 30.13 (2018): 1705791. https://doi.org/10.1002/adma.201705791

### Comping LAMMPS with GraFF

1. Copy the pair_hbond_graphene.cpp and pair_hbond_graphene.h into your LAMMPS src directory.
2. Add `#include "pair_hbond_graphene.h"` to the style_pair.h file, which should also be in the src directory
3. Recompile LAMMPS in the usual way

When first developing GraFF we used the 16-Feb-2016 version of LAMMPS, I see no reason why any other version would interfere with GraFF.

### Using the GraFF pair_style in a LAMMPS Simulation

Only the graphitic carbon atoms should use the GraFF 3-body potential, all others, in our simulatinos, used the lj/coul/cut/long. To overlay these interactions we must use the pair hybrid/overlay command. When using this command you must explicitly pass LAMMPS each atomic pair interaction because it cannot use the usual mixing rules.

The pair command is:
```
pair_style hybrid/overlay lj/cut/cou/long outer_cutoff inner_cutoff hbond_graphene cos(x)_power inner_cutoff outer_cutoff minimum_angle
```

Initialising a system with only graphitic carbons would then require these lines:
```
pair_style hybrid/overlay lj/cut/coul/long 11.0 9.0 hbond/graphene 2 5.0 6.0 45
pair_coeff  1 1 hbond/graphene 1 i 0.025  3.627 2
pair_coeff  1 1 lj/cut/coul/long 0.020 3.5500
```

We rocommend using the OPLS forcefield for bonded interactions and any other molecule interactions in the simulation: DOI:10.1021/ja9621760

### Example

We provide a minimal example to test your GraFF intsalation with the files `in.example` and `graphene.data`. In the example atom type 1 is Carbon, 2 is hydrogen.

`lmp_exec < in.example` should run if your intsall is correct.

Contact me github if you have any issues.
 
