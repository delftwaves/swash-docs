run instructions
================

SWASH can be run on various architectures, including laptops, HPC machines and cloud-based clusters,
Depending on your choice of :ref:`installed SWASH <install>`, you can run the simulations locally on an OS platform (Windows, Linux or Mac)
via your own built SWASH or via Docker. Additionally, you can run SWASH on HPC with the SWASH docker image using Apptainer or Singularity.

usage of built SWASH
--------------------

local copy of SWASH
~~~~~~~~~~~~~~~~~~~

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

from Docker image
~~~~~~~~~~~~~~~~~

The user can either choose between running SWASH directly using docker or the docker in interactive mode (option ``-it``).

To run SWASH directly, copy and paste the following command, replace the required run parameters, and hit Enter::

   docker run --rm -v .:/home/swash delftwaves/swash swashrun -input <SWASH-command-file-without-extension> -mpi <nprocs>

To run the SWASH container interactively, enter::

   docker run --rm -v .:/home/swash -it delftwaves/swash bash

Once the interactive bash shell is started in the container, the user can access the commands available from Linux Ubuntu (e.g., ``ls``, ``find``,
``which``) and SWASH (e.g., ``swashrun``, ``swash.exe``).

.. note::

   - The option ``-v .:/home/swash`` ensures that the SWASH output files and the PRINT file created in the directory ``/home/swash`` of the
     docker container will store in your local current directory.
   - The option ``--rm`` removes the exited container from your machine after terminating SWASH. (Check by invoking the command ``docker ps -a``.)
   - For a complete overview of the command ``docker run``, please refer to this `page <https://docs.docker.com/reference/cli/docker/container/run/>`_.

use with Apptainer or Singularity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `SWASH docker image <https://hub.docker.com/r/delftwaves/swash>`_ can be used with `Apptainer <https://apptainer.org>`_ and
`Singularity <https://sylabs.io/singularity/>`_.
These container platforms are especially useful for running SWASH on HPC clusters, either locally or in the cloud
(e.g., `Microsoft Azure <https://azure.microsoft.com/en-us>`_ and `Amazon Web Services <https://aws.amazon.com/>`_).

First, pull the docker image in the following way::

   apptainer pull docker://delftwaves/swash:latest

This pull creates a singularity image named ``swash_latest.sif``, which can then be run with standard Apptainer commands. For example::

   apptainer run --bind .:/home/swash swash_latest.sif swashrun -input <SWASH-command-file-without-extension> -mpi <nprocs> > swashout &

where ``<SWASH-command-file-without-extension>`` is the name of your command file without extension and ``<nprocs>`` indicates
how many processors need to be launched for a parallel MPI run.

To interactively execute commands within the SWASH container, enter::

   apptainer shell swash_latest.sif

Consult this `page <https://apptainer.org/docs/user/main/quick_start.html#overview-of-the-apptainer-interface>`_ to explore the possibilities with Apptainer.

.. note::

   Apptainer is compatible with Singularity, implying that you may use the command ``singularity`` instead of ``apptainer``.
