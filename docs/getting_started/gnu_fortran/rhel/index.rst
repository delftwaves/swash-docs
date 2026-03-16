install SWASH on RPM-based Linux distributions
==============================================

.. _prerequisiteslr:

prerequisites
-------------

The following packages must be installed first:

- gfortran
- cmake
- ninja
- perl
- git

These packages can be installed using the default package manager ``dnf``.

First, update the system

.. code-block:: bash

   sudo dnf -y update

Next, run the following command to install GCC (GNU Compiler Collection) on the system

.. code-block:: bash

   sudo dnf -y install gcc

and then ``gfortran``

.. code-block:: bash

   sudo dnf -y install gcc-gfortran

To install ``cmake``, enter

.. code-block:: bash

   sudo dnf -y install cmake

Finally, install ``ninja`` by running the following command

.. code-block:: bash

   sudo dnf -y install ninja-build

.. warning::

   On Rocky Linux 9+, this command must be preceded by the two commands below:

   .. code-block:: bash

      sudo dnf -y install dnf-plugins-core
      sudo dnf config-manager --set-enabled crb

   Note that Rocky 8 installs an older version of Ninja, namely 1.8.2, while ``CMake`` requires 1.10+ in order to build fortran. So if possible, please upgrade to Rocky 9 or higher.

Before installing ``perl``, check if it is already present on your Linux distribution, by typing

.. code-block:: bash

   perl -v

If ``perl`` is not installed, the shell reports that this command is not found.
In that case, install the perl interpreter as follows

.. code-block:: bash

   sudo dnf -y install perl

The same for ``git``. Either verify the installation by typing ``git version`` or install it by running ``sudo dnf -y install git``.

verify installations
~~~~~~~~~~~~~~~~~~~~

Verify the required installations by checking their versions, as follows

.. code-block:: bash

   gfortran --version

   cmake --version

   ninja --version

   perl --version

   git --version

If no error is reported, then the installation was successful.

.. important::

   - The ``CMake`` version must be at least 3.20 or newer.
   - The ``ninja`` version should be at least 1.10.
   - The ``perl`` version is 5 or higher.

installation SWASH
------------------

Once the prerequisites are taken care of, installing SWASH on your machine is a four-step process.

1. download SWASH

.. code-block:: bash

   git clone https://gitlab.tudelft.nl/citg/wavemodels/swash.git && cd swash

Paste this into a shell terminal.

2. configure SWASH

.. code-block:: bash

   make config

3. build SWASH

.. code-block:: bash

   make

4. install SWASH

.. code-block:: bash

   make install

SWASH is installed at folder ``$HOME/wavemodels/swash`` by default.
To run SWASH, you need to make sure that the ``/bin`` folder in this directory is added to your system's ``PATH``.
Open the terminal and enter

.. code-block:: bash

   export PATH=$PATH:$HOME/wavemodels/swash/bin

You can check the new value of ``PATH`` by echoing it: ``echo $PATH``.
However, to set this permanently, you need to add it to your ``~/.bashrc``, as follows

.. code-block:: bash

   echo export PATH=$PATH:$HOME/wavemodels/swash/bin >> ~/.bashrc
   source ~/.bashrc

options for configuring SWASH
-----------------------------

If desired, the build can be configured by passing one or more options below to ``make config``.

    ===================  ==================================================================
    ``fc=<compiler>``    the Fortran90 compiler to use [default is determined by ``CMake``]
    ``mpi=on``           enable build of SWASH with MPI [``off`` by default]
    ``prefix=<folder>``  set the installation folder [``$HOME/wavemodels/swash by`` default]
    ===================  ==================================================================

For example, the following command

.. code-block:: bash

   make config fc=gfortran prefix=/usr/local/swash

will configure SWASH to be built using ``gfortran`` and then install it at ``/usr/local/swash``.

.. _bmpir:

building with MPI support
-------------------------

The SWASH source code also supports memory-distributed parallelism for high performance computing applications.
A message passing approach is employed based on the Message Passing Interface (MPI) standard that enables communication between independent processors.

Popular implementations are `Open MPI <https://www.open-mpi.org>`_ and `MPICH <https://www.mpich.org>`_.
The first one is typically offered by the package managers of Linux and macOS and can be combined with GCC such as gfortran.

Before installing Open MPI, make sure that your system is up to date and that GCC has been installed, see :ref:`prerequisites <prerequisiteslr>`.

To install Open MPI on a RPM-based Linux, run

.. code-block:: bash

   sudo dnf -y install openmpi openmpi-devel

To verify whether the installation was successful, run the following command

.. code-block:: bash

   ompi_info --version

or

.. code-block:: bash

   mpirun --version

.. warning::

   To use the compiler ``mpifort`` and runner ``mpirun`` on Fedora and Rocky Linux, you'll need to set up the environment path.
   Insert the following commands

   .. code-block:: bash

      echo source /etc/profile.d/modules.sh >> ~/.bashrc
      echo module load mpi/openmpi-$(arch) >> ~/.bashrc
      source ~/.bashrc

Once Open MPI is operational, we proceed to build SWASH. First, we configure SWASH to be built with support for Open MPI, as follows

.. code-block:: bash

   make config fc=mpifort mpi=on

The actual building is done by typing

.. code-block:: bash

   make

Finally, to install SWASH, run the following command

.. code-block:: bash

   make install

SWASH is now ready for high performance computing.

clean up
--------

To remove all files installed by ``make install``, type the following command

.. code-block:: bash

   make uninstall

If you want to remove the build directory and all files that have been created after running ``make`` or ``make build``, then run

.. code-block:: bash

   make clobber
