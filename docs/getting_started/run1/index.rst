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

   - Open an :ref:`Intel oneAPI command prompt <ioap>` in Windows if you have installed the :ref:`Intel Fortran Compiler <intelwin>`.
   - In case of Linux/macOS, the script ``swashrun`` needs to be made executable first, as follows::

       chmod +rx swashrun

To redirect screen output to a file, use the sign ``>``. Use an ampersand to run SWASH in the background. An example::

   swashrun -input mytest > swashout &

where ``mytest.sws`` is your SWASH command file.

.. note::

   To run SWASH on Windows in the background, type the built-in ``start`` command in command prompt, as follows::

      start "" /min cmd /c "swashrun mytest > swashout 2>&1"

For a parallel MPI run, you must specify the number of processors ``<nprocs>`` that will be launched, as follows:

.. code-block:: text

   swashrun <SWASH-command-file-without-extension> <nprocs>

in case of Windows, or

.. code-block:: bash

   swashrun -input <SWASH-command-file-without-extension> -mpi <nprocs>

in case of Linux/Mac. By default, ``nprocs = 1``.

As an example, assume that the command file is named ``mysim.sws`` while the corresponding simulation needs to be run on 5 cores.
In case of Windows, type the following command in an :ref:`Intel oneAPI command prompt <ioap>`::

   start "" /min cmd /c "swashrun mysim 5 > swashout 2>&1"

while run the following command in a Linux/Mac terminal::

   swashrun -input mysim -mpi 5 > swashout &
