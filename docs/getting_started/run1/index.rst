local copy of built SWASH
=========================

The provided run scripts (``swashrun`` and ``swashrun.bat``) enable the user to properly and easily run SWASH both serial as well as parallel.

For Windows users, open a terminal (hit the Window key + R, type ``cmd`` and click OK), navigate to the folder containing your SWASH input files
(command file, grid, bathymetry, etc.), copy and paste the following command, then replace ``<SWASH-command-file-without-extension>``
by the name of your SWASH command file without extension (assuming it is ``sws``), and hit Enter::

   swashrun <SWASH-command-file-without-extension>

while for Linux and Mac users, type in an opened terminal the following command::

   swashrun -input <SWASH-command-file-without-extension>

.. important::

   The script ``swashrun`` needs to be made executable first, as follows::

     chmod +rx swashrun

To redirect screen output to a file, use the sign ``>``. Use an ampersand to run SWASH in the background. An example::

   swashrun -input mytest > swashout &

For a parallel MPI run, you must specify the number of processors ``<nprocs>`` that will be launched, as follows:

.. code-block:: text

   swashrun <SWASH-command-file-without-extension> <nprocs>

in case of Windows, or

.. code-block:: bash

   swashrun -input <SWASH-command-file-without-extension> -mpi <nprocs>

in case of Linux/Mac. By default, ``nprocs = 1``.
