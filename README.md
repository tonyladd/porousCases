These are some sample cases for porousFoam v3, a code for porous media
dissolution that includes a Darcy-Brinkman flow solver. It is designed
to have user programmable constitutive models, examples of which are
in the Models subdirectory.

The velocity field from the Darcy solver is not conservative. There
are typically significant oscillations in U when the permeability
is strongly varying. These might be made smaller by using different
grad schemes, for example "Gauss cubic" instead of "Gauss linear"
or "cellLimited Gauss linear 1".
However the phi field used in the transport equation is conservative
and the concentration fields are unaffected.

The cases can be executed with the Allrun script: cases 3 and 5
are configured to run in parallel (4 processors) while the others
will run in serial mode.

Cases 1-4 were run with reaction limited kinetics (reacLin.H)
while cases 5 and 6 were run with the two component model for roll
front type kinetics (reacRoll.H),

1) slot: Concentration fields in a rectangular slot. Comparison of
    the convergence of linearUpwind and upwind schemes with dissol
    These tests used P**10 perm law

2) darcy2D: Small seed of higher porosity (cubic perm)
    Initial porosity is 0.1 with seed of porosity 1 (K/K0=1000)
    Checked C (c) and P (h) with dissol: in the case directory
    compare C_10.png with c_0100.png and P_10.png with h_0100.png
    porousFoam (32) more than 10X dissol+MUMPS (290s)
    dissol+AMG was even slower (760s)
    Note: In porousFoam P varies from 0.1 to 1, whereas h varies
    from 1-10.  Hence the time scale in dissol is 10X and Pe = 10
    (instead of 1) because of the factor of h or P in D
    The time scale in porousFoam (no k0 in the dissolution)
    accounts for the other factor of 10.

3) brinkman2D: Small seed of higher porosity (cubic perm)
    Same as case 1) but with Darcy-Brinkman flow
    K0 controls the contribution of Darcy drag and viscous stress
    The pressure field should be adjusted so the initial velocity is
    close 1 to 1, meaning Uscale ~ 1. This helps the convergence
    A useful rule for underrelaxation in SIMPLE is that
    prelax + Urelax = 1. It can be larger but soon it will become
    unstable. Its best to adjust U and p to relax both fields at about
    the same rate.

4) porous3D: Wormhole formation in a layered medium (cubic perm)
    The matrix has an initial porosity of 0.1 with a layer near the
    center of lower porosity (0.01). A small seed near the inlet
    initiates the wormhole. I made movies of the porosity: a slice
    (P.ogv) and a contour (P3D.ogv). It seems necessary to use
    upwind differencing in this case because of the low resolution.

5) roll1D: 1D roll front
    In comparison with Pawel's rollD0.ogv, which uses a constant
    diffusion coefficient, roll1D.ogv uses the consitutive law D = D0*P.

6) roll2D: 2D roll front
    Illustrating the instability in 2D

Source codes are in porousFoam (v3.1 + OpenFOAM-4.x)
