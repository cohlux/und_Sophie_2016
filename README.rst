=====
pySRU
=====

WARNING: PROJECT UNDER DEVELOPMENT!!

Synchrotron Radiation Undulator emission in python

Main development website: https://github.com/vasole/fisx

A toolbox to calculate the emission of radiation by undulators in storage rings. 


This software library implements formulas to calculate the emission of radiation by undulators in storage rings, and more
generally, the emission of a relativistic electron in an arbitrary magnetic field.

The software is written in pure python, using standard packages as numpy and scipy. It uses matplotlib for plots, but it is not required for calculations.

License
-------

This code is relased under the MIT license as detailed in the LICENSE file.

Installation
------------

To build the library for python use, just use the ``python setup.py install`` approach.

Testing
-------

To run the tests **after installation** run::

    python -m pySRU.tests.testAll

Example
-------

The file Simulations.py contains a number of simulations. For example the following code created the emission of an undulator like in fig 2.5 of the X-ray Data Booklet (http://xdb.lbl.gov/Section2/Sec_2-1.html)

There is a `web application <http://fisxserver.esrf.fr>`_ using this library for calculating expected x-ray count rates.

This piece of Python code shows how the library can be used via its python binding.

.. code-block:: python

    from  pySRU.ElectronBeam import ElectronBeam

    print("======================================================================")
    print("======      Undulator from X-ray data booklet                  =======")
    print("====== fig 2.5 in  http://xdb.lbl.gov/Section2/Sec_2-1.html    =======")
    print("======================================================================")

    # note that the flux in the reference fig 2.6 is a factor 10 smaller than the calculated here.
    # This factor comes from the units:
    #     here: phot / s  / A / 0.1%bw / (mrad)^2
    #     ref : phot / s  / A /   1%bw / (0.1 mrad)^2

    undulator_test = Undulator(K=1.87, period_length=0.035, length=0.035 * 14)
    electron_beam_test = ElectronBeam(Electron_energy=1.3, I_current=1.0)

    simulation_test = create_simulation(magnetic_structure=undulator_test,electron_beam=electron_beam_test,
                        magnetic_field=None, photon_energy=None,
                        traj_method=TRAJECTORY_METHOD_ANALYTIC,Nb_pts_trajectory=None,
                        rad_method=RADIATION_METHOD_APPROX_FARFIELD, Nb_pts_radiation=101,
                        initial_condition=None, distance=None,XY_are_list=False,X=None,Y=None)


    simulation_test.print_parameters()

    simulation_test.trajectory.plot_3D(title="Electron Trajectory")

    simulation_test.radiation.plot(title="Flux in far field vs angle")

    #  second calculation, change distance and also change the grid to accept the central cone
    simulation_test.change_distance(D=100)
    simulation_test.calculate_on_central_cone()

    simulation_test.radiation.plot(title="New distance: %3.1f m"%simulation_test.radiation.distance)