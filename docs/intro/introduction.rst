What is SWASH?
==============

introduction
------------

SWASH (an acronym of Simulating WAves till SHore) is a non-hydrostatic wave-flow model and is
intended to be used for predicting transformation of dispersive surface waves from offshore to the beach
for studying the surf zone and swash zone dynamics, wave propagation and agitation in ports and harbours,
rapidly varied shallow water flows typically found in coastal flooding resulting from e.g. dike breaks,
tsunamis and flood waves, density driven flows in coastal waters, and large-scale ocean circulation,
tides and storm surges.

The basic philosophy of the SWASH code is to provide an
efficient and robust model that allows a wide range of time and space scales of surface waves and shallow
water flows in complex environments to be applied.
The governing equations are the nonlinear shallow water equations including non-hydrostatic pressure, and
optionally the equations for conservative transport of salinity, temperature and suspended sediment.
In addition, the vertical turbulent dispersion of momentum and diffusion of salt, heat and sediment load
are modelled by means of the standard *k*-:math:`{\varepsilon}` turbulence model. The transport equations
are coupled with the momentum equations through the baroclinic forcing term, while the equation of state is
employed that relates density to salinity, temperature and sediment.

The need to accurately predict small-scale coastal flows and transport of contaminants encountered in environmental issues
is becoming more and more recognized. In principle, SWASH has no limitations and can capture flow phenomena with spatial
scales from centimeters to kilometers and temporal scales from seconds to hours. Yet, this model can be employed to resolve
the dynamics of wave transformation, buoyancy flow and turbulent exchange of momentum, salinity, heat and suspended
sediment in shallow seas, coastal waters, estuaries, reefs, rivers and lakes. Examples are small-scale coastal applications,
like waves approaching a beach, wave penetration in a harbour, flood waves in a river, oscillatory flow through canopies,
salt intrusion in an estuary, internal waves, and large-scale ocean, shelf and coastal systems driven by Coriolis and
meteorological forces to simulate tidal waves and storm surge floods.

It should be emphasized that SWASH is not a Boussinesq-type wave model.
Conceptually, the vertical structure of the flow is a part of the solution.
In fact, SWASH may either be run in depth-averaged mode or multi-layered mode in which the computational domain
is divided into a fixed number of vertical terrain-following layers. SWASH improves its frequency dispersion by
increasing this number of layers rather than increasing the order of derivatives of the dependent variables like
Boussinesq-type wave models do. Yet, SWASH contains at most second order
spatial derivatives, whereas the applied finite difference approximations are at most second order accurate
in both time and space. This is probably the main reason why SWASH is much more robust and faster than any other
Boussinesq-type wave model. This approach receives good linear frequency dispersion
up to :math:`kh \le 7` with two equidistant layers at 1% error in phase
velocity (*k* and *h* are the wave number and water depth, respectively).
In addition, SWASH does not have any numerical filter nor dedicated
dissipation mechanism to eliminate short wave instabilities.
Neither does SWASH include other ad-hoc measures like the surface roller model for wave breaking, the slot
technique for moving shoreline, and the alteration of the governing
equations for modelling wave-current interaction. See also an interesting
`paper <https://www.waveworkshop.org/12thWaves/papers/Zijlema_12thWaveWorkshop_Nov2011.pdf>`_ on this subject.

SWASH is close in spirit to `SWAN <https://swanmodel.sourceforge.io>`_ with
respect to the pragmatism employed in the development of the code in the sense that comprises are sometimes
necessary for reasons of efficiency and robustness. Furthermore, like SWAN, the software package of SWASH includes
user-friendly pre- and post-processing and does not need any special libraries.
In addition, SWASH is highly flexible, accessible and easily extendible
concerning several functionalities of the model.

`Applications <https://swash.sourceforge.io/examples/examples.htm>`_ drawn from the work of the Fluid Mechanics research
group at Delft University convey an impression of the capabilities of SWASH.
Also, many `scientific papers, reports and other documents <https://swash.sourceforge.io/references/references.htm>`_
on SWASH have been published.

physics
-------

SWASH accounts for the following physical phenomena:

* wave propagation, frequency dispersion, shoaling, refraction and diffraction
* nonlinear wave-wave interactions (including surf beat and triads)
* depth-limited wave growth by wind
* wave breaking
* wave runup and rundown
* moving shoreline
* bottom friction
* partial reflection and transmission
* wave interaction with rubble mound structures
* wave interaction with floating objects
* wave-current interaction
* wave-induced currents
* vertical turbulent mixing
* subgrid turbulence
* turbulence anisotropy
* wave damping induced by aquatic vegetation
* rapidly varied flows
* tidal waves
* bores and flood waves
* wind driven flows
* space varying wind and atmospheric pressure
* geophysical fluid flows
* density driven flows
* transport of suspended load for (non)cohesive sediment
* turbidity flows
* transport of tracer

computations
------------

SWASH computations can be made on a regular, an orthogonal curvilinear grid and a triangular mesh in a Cartesian or spherical coordinate system.

SWASH runs can be done serial, that is, one SWASH program on one processor, as well as parallel, that is, one SWASH program on more than
one processor using an MPI protocol.

boundary conditions
-------------------

SWASH provides the following specification of boundary conditions:

* wavemakers:

  - regular waves by means of Fourier series or time series
  - irregular unidirectional waves by means of 1D spectrum

    the spectrum may be obtained from observations or by specifying a parametric shape (Pierson-Moskowitz, Jonswap or TMA)
  - irregular multidirectional waves by means of 2D spectrum. The spectrum may be obtained from a SWAN run or by specifying a parametric shape (Pierson-Moskowitz, Jonswap or TMA) while the directional spreading can be expressed with the well-known cosine power or in terms of the directional standard deviation

* absorbing-generating boundary conditions
* velocity or discharge
* Riemann invariants
* full reflection at closed boundaries or solid walls
* Sommerfeld or radiation condition
* sponge layers
* periodic boundaries

output quantities
-----------------

SWASH provides the following output quantities (ASCII or binary Matlab files containing tables and maps):

* surface elevation
* depth-averaged velocity magnitude and direction
* layer-averaged velocity magnitude and direction
* vertical distribution of horizontal velocity
* velocity in z-direction or relative to sigma plane
* time-averaged velocities (e.g. undertow)
* discharges
* friction velocity
* vorticity
* turbulence quantities (*k*, :math:`{\varepsilon}` and :math:`{\nu}_t`)
* time-averaged turbulence quantities
* transport constituents (salt, heat and suspended sediment)
* time-averaged transport constituents
* pressure at bottom
* layer-averaged pressure
* vertical distribution of pressure
* non-hydrostatic pressure
* hydrodynamic loads acting on a moored ship
* significant wave height
* wave-induced setup
* maximum horizontal runup or inundation depth
* vertical runup height

limitations
-----------

At present, SWASH does not account for:

* hotstarting (*temporary limit*)
* unstructured mesh computations in parallel using MPI (*temporary limit*)
