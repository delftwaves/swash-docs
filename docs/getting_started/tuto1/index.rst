.. _tuto1:

tutorial 1: run SWASH in docker container
=========================================

This is a tutorial to get you started with setting up SWASH and then using it to model breaking waves propagating on a plane beach.

In this tutorial we assume you already have some basic knowledge of ocean waves.
It's also helpful to have access to Matlab or Jupyter Notebook for post processing later in this tutorial.

step 1: install Docker
----------------------

To keep things as simple as possible, we're not going to install SWASH and then start working with it from scratch. Instead, we'll run SWASH in the Docker container.
Part of the intended model has already been set up and is resided in the container while the rest of the model will be set up by you in this tutorial.

If you haven't installed Docker yet, you can download it from the official `Docker website <https://www.docker.com/get-started>`_.

Once you have Docker installed in your system, verify the installation by checking its version:

.. code-block:: bash

   docker --version

If no error is reported, then the installation was successful.

step 2: pull SWASH tutorial image
---------------------------------

Open a terminal and run the following command to pull the docker image specifically developed for this tutorial:

.. code-block:: bash

   docker pull delftwaves/swash-tutorial && docker tag delftwaves/swash-tutorial waves

.. note::

   The image pulled from the `Docker Hub <https://hub.docker.com>`_ is termed as ``delftwaves/swash-tutorial``.
   However, to keep typing short, it is tagged as ``waves``.
   This name is arbitrary so you can give it a different name if you want.

Let's check whether the obtained image is on your system by listing the available images:

.. code-block:: bash

   docker image ls

This command shows the images, their repository and tags, and their size.

step 3: create working directory
--------------------------------

Once the docker image is pulled, create a working directory called ``swash`` in your terminal and then navigate to it:

.. code-block:: bash

   mkdir swash && cd swash

step 4: setup input files
-------------------------

Inside the folder ``swash`` two input files will be created.
These files are part of the model we use to simulate irregular waves on the beach with variable depth.
The first file defines the bathymetry. For the purpose of this container the filename must be ``bathy.txt``.

.. important::

   This naming convention is not a limitation of SWASH. For regular run simulations, the user is free to assign custom, yet functional, names to the input fields such as
   bathymetry and currents.

The model is partially set up in the container and it consists of a 1D domain with a length of 1 km.
The seabed level ranges from 10 m below still water level to 2 m above this level.
Hence, the beach slope is approximately 1:100.

Create or edit the file ``bathy.txt`` and insert the following two lines:

.. code-block:: none

   10. -2.

.. note::

   The bed level is defined positively downward from the still water level :math:`z = 0` (in the conventional Cartesian coordinate system).

The second file concerns the specification of the boundary conditions.
The mandatory filename is ``bc.txt``. (Again, this naming is only meant for the container.) Create this file and specify the boundary conditions in the following way:

.. code-block:: none

   BOUND SHAPE JONSWAP SIG PEAK
   BOUND SIDE West BTYPE WEAK ADDBoundwave CON SPECTrum 0.5 10. cycle 20 min

What does this mean? We impose irregular waves to the west side of the model domain using a Jonswap spectrum with a significant wave height of 0.5 m and a peak period of 10 s.
Furthermore, second order sub- and super-harmonic bound waves are added to these first order free waves. Finally, for the synthesization of a time series of the surface
elevation at the boundary a cycle period of 20 minutes is employed.

.. note::

   For a correct syntax specification of boundary conditions and the various options, click on this `page <https://swash.sourceforge.io/online_doc/swashuse/swashuse.html#dx1-34003>`_.

.. _step5:

step 5: run docker container
----------------------------

Now it is time to run SWASH inside container ``waves``. First, ``cd`` to your working directory ``swash``, and then insert the following command in your terminal:

.. code-block:: bash

   docker run --name myfirstrun -v .:/home/swash waves

.. note::

   - The parameter ``myfirstrun`` is an identifier for the container run. It is helpful to give each run a different name so you can keep track of which runs have been completed.
     Use the command ``docker ps -a`` for this.
   - The ``-v`` directive will allow data to be read or written from or to the host system. The syntax is ``-v <hostdir>:<containerdir>``.
     Here, ``hostdir`` is your current directory ``.`` and ``containerdir`` is the directory ``/home/swash`` in the container.

You'll now see the simulation progress on your screen. After a while, the simulation will finish, and *SWASH-in-container* will have generated two output files: ``PRINT`` and ``output.txt``.
The ``PRINT`` file contains the echo of the input, information concerning the parameters used, possible warning and errors, etc.
The other file contains the actual model results of the simulation.

In addition, the container has prepared two other files for you related to post processing: a Matlab file called ``mkplot.m`` and a Jupyter Notebook ``mkplot.ipynb``.

.. _step6:

step 6: post processing
-----------------------

File ``output.txt`` contains a table with four columns. Each column represents a specific output parameter. These are:

#. the distance from the beginning of the computational domain (that is, offshore),
#. the bottom level,
#. the significant wave height, and
#. the wave setup,

respectively.

Plots can be created using either Matlab or Jupyter Notebook.

Matlab
^^^^^^

- start Matlab:

  - via terminal:

    first, navigate to your working directory ``swash`` and then insert

    .. code-block:: bash

       matlab

  - Windows:

    - use the `Start menu` and click on `MATLAB`, or
    - double-click the file `mkplot.m`

  - macOS:

    - type ``matlab`` in the terminal, or
    - open `Applications` folder and double-click `MATLAB`

- run the script:

  in `Command Window`, type

  .. code-block:: matlab

     mkplot

Jupyter Notebook
^^^^^^^^^^^^^^^^

#. Open a terminal, navigate to your working directory ``swash``, and then type

   .. code-block:: bash

      jupyter notebook mkplot.ipynb

   You should see the notebook open in your browser.
#. Execute the code cells one by one.


step 7: are you satisfied with the results?
-------------------------------------------

Do the results agree with the theoretical expectations?

.. tip::

   - Try a different beach slope or a different wave height.
   - Always check the ``PRINT`` file for warnings and errors!

Once you are satisfied, you may continue with the next :ref:`tutorial <tuto2>`.
