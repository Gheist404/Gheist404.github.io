---
layout: post
title:  "modelling the BBB"
date:   2026-02-28
categories: synbio
---

# Introduction

For Sheffield's iGEM 2026 team we aim to be able to pass the blood brain barrier to be able to find a way for drug delivery to the brain, an important part of this is to be able to model this complex system so with the use of research available here is hopefully the template as described by [Carpenter et. al, 2014](#carpenter)

# Process

Goal is to find effective permeability to be able to find the BBB permeability. In order to do this need to put the desired compound (in this situation at some point a custom exosome) through a 1,2-dioleoyl-sn-glycero-3-phosphocholine bilayer to find potential mean of force (PMF)

Based on large assumption a compound being able to pass through the BBB is based on it's BBB permeability, CNS+ meaning it will pass and CNS- meaning it won't. This BBB permeability stuff can sometimes be slightly unclear but there are 2 measures:
* logBBB, the ratio of the drug in the blood to the ratio in the brain
* logPS, permeability surface area product, which means that this will also consider the speed of the uptake of the drug to unlike logBBB which is just at equilibrium

For the model we will use molecular dynamics to be able to predict if a compound can pass. This will use <b>free-energy simulations</b> through a <b>simple BBB mimic</b> and be able to get the <b>effective permeability</b>

# Method

first of all I want to check that the paper's method will actually work so I am going to try and get a similar result for atenolol but first I need to get/make my ingredients for making the program work:
* GROMACS
* Berger et al force field &larr; <i>BBB Mimic (DOPC molecules)</i>
* gromos53a6 force field for small compounds 
* SPC model for water

Currently I know the Berger et al force field has been taken offline due to lack of accuracy so I would like to get a copy of it but also try it with a new and improved version.

Steps as stated in [Carpenter et. al, 2014](#carpenter):
1. single copy of compound <i>(exosome)</i> harmonically restrained, at specific locations along z axis in solvated Berger et al force field. This gave the system 5124 water molecules, 72 DOPC molecules and one compound.
2. system energy minimised using a mixture of steepest descentes and conjugate gradients:
    - performed at 323K, using the Nose-Hoover thermostat with &#120591;<sub>T</sub> = 0.5 ps
    - pressure maintained at 1 bar, using semi-isotropic Parrinello-Rahman barostat with with &#120591;<sub>T</sub> = 1.0 ps
    - compressibility of 4.5 x 10<sup>-5</sup>bar<sup>-1</sup>
    - bond lengths constrained, using LINCS algorithm, allowing a 2 fs time step to be used
    - Nonbounded interactions were truncated at 1.4nm and the neighbour list updated every 10 ps
    - Long range electrostatic interactions were calced using the particle mesh Elwald method
    - Automated Topology Builder server for making the compound fit
        - Optimised at the HF/STO-3G level of theory
        - reoptimised at the B3LYP/6-31G level of theory in implicit water
        - charges initially estimated by fitting the electrostatic potential using the Kollman-Singh scheme
        - Then the hessian matrix is used
3. Free energy calcs
    - Umbrella sampling for PMF
    - a single harmonic restrain with a force constant of 1000KJ/mol/nm was applied to the distance between the center of mass of the DOPC bilayer and the center of mass of the compound in the direction of the z axis.
    - 100 different umbrella sampling sims were performed in increments of 1nm along the z axis direction
        - DOPC treated as z=0
        - so spread between z=-5 and z=5


# Appendix
- <div id="carpenter"><a href="https://www.sciencedirect.com/science/article/pii/S000634951400664X#bbib35">Method to Predict Blood-Brain Barrier Permeability of Drug-Like Compounds Using Molecular Dynamics Simulations</a></div>
