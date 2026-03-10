.. _tuto2:

tutorial 2: the command file
============================

In this tutorial we will learn the basic input of SWASH and how to use it.

At the basis of SWASH input is the so-called *command file* which contains some essential commands.
These commands include the following components:

- create/read the computational mesh domain,
- read input data such as the bathymetry and currents,
- impose boundary conditions,
- set some physical and numerical parameters, and finally
- define the output locations and output parameters.

For a list of available commands, click `here <https://swash.sourceforge.io/online_doc/swashuse/swashuse.html#description-of-commands>`_.

step 1: download command file
-----------------------------

In the previous tutorial, we ran one or more simulations of breaking waves propagating over a beach with variable depth. The corresponding command file is resided in the container ``waves``.
The base name of this file is ``INPUT``.

As a first step, we retrieve this ``INPUT`` file from the container. Navigate to folder ``swash`` and run the following command:

.. code-block:: bash

   docker cp myfirstrun:/home/user/INPUT .

.. attention::

   If you specified a different ID run than ``myfirstrun`` in the previous tutorial (see :ref:`step 5 <step5>`), please specify this in the command above.

step 2: modify ``INPUT``
------------------------

Edit the file ``INPUT`` and replace the following line::

  READINPUT BOTTOM 1. '../swash/bathy.txt' 1 0 FREE

by this line::

  READINPUT BOTTOM 1. 'bathy.txt' 1 0 FREE

Do this for the next line as well::

  include '../swash/bc.txt'

must be replaced by::

  include 'bc.txt'

.. tip::

   Take a look at the command file and see if you already understand any of the commands. You can always consult the
   `manual <https://swash.sourceforge.io/online_doc/swashuse/swashuse.html#index>`_ to find out exactly what all these
   commands mean and how to specify them.

step 3: rename the file ``INPUT``
---------------------------------

For SWASH runs, it is better to use a clear yet functional filename instead of ``INPUT``. However, the file extension must be set to ``.sws``.
In this tutorial we will give the name ``wavbrk.sws`` for the test case to be discussed.

Depending on your local OS, run one of the following commands:

- Windows:

  .. code-block:: bat

     ren INPUT wavbrk.sws

- Linux / macOS:

  .. code-block:: bash

     mv INPUT wavbrk.sws

We will now repeat the simulation from the previous :ref:`tutorial <tuto1>`, but this time with the command file ``wavbrk.sws``, which is located on your machine.
Make sure that all three files, i.e. ``wavbrk.sws``, ``bathy.txt`` and ``bc.txt``, are in the same folder.

The default script to perform a SWASH run is called ``swashrun`` and the instruction syntax is as follows::

  swashrun -input <SWASH-command-file-without-extension> -mpi <nprocs>

with ``<SWASH-command-file-without-extension>`` the name of the command file without extension and ``<nprocs>`` indicating the
number of processors to be launched for a parallel MPI run.

The syntax shown above applies only to Linux and Mac. For Windows, the run syntax is as follows:

.. code-block:: bat

   swashrun <SWASH-command-file-without-extension> <nprocs>

step 4: rerun the previous simulation
-------------------------------------

The run script ``swashrun`` is inside the container which has a Linux kernel pre-installed.
To run our own simulation, execute the following command:

.. code-block:: bash

   docker run -v .:/home/user waves swashrun -input wavbrk > swashout &

.. note::

   - The ``--name`` flag is actually no longer needed because run identification is now done via the command file.
   - The run progress is not displayed on the screen but written to ``swashout``.

Repeat the :ref:`post processing step <step6>` from the previous tutorial.

Once you are done with this step you can move on to the next :ref:`tutorial <tuto3>`.
