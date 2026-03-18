install SWASH on Debian-based Linux distributions
=================================================

.. _prerequisitesldi:

prerequisites
-------------

The following packages must be installed first:

- gcc
- cmake
- ninja
- git
- perl
- wget
- gnupg
- intel-fortran-essentials

These packages can be installed using the package manager ``apt``.

Open a command line terminal and run the following commands:

.. code-block:: bash

   sudo apt -y update

followed by

.. code-block:: bash

   sudo apt -y install build-essential cmake ninja-build git wget gnupg

.. note::

   The ``build-essential`` package installs essential tools and libraries for compiling the source code, including ``gcc`` and ``make``.

The Linux flavors Debian, Ubuntu and Mint have ``perl`` installed by default.

The final step is to install the Intel Fortran Essentials package which also includes the MPI libraries. First, set up the Intel repository
by downloading the key to system keyring:

.. code-block:: bash

   wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB \
   | gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null

followed by adding signed entry to apt sources and configure the APT client to use Intel repository:

.. code-block:: bash

   echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | sudo tee /etc/apt/sources.list.d/oneAPI.list

Next, update the repository index:

.. code-block:: bash

   sudo apt update

Finally, install the package with the following command:

.. code-block:: bash

   sudo apt -y install intel-fortran-essentials

Let your OS system know where to find the compilers and libraries:

.. code-block:: bash

   echo export INTF=/opt/intel/oneapi >> ~/.bashrc
   echo "source \$INTF/setvars.sh > /dev/null 2>&1" >> ~/.bashrc
   source ~/.bashrc

verify installations
~~~~~~~~~~~~~~~~~~~~

Verify the required installations by checking their versions, as follows

.. code-block:: bash

   ifx --version

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

   echo export PATH=$PATH:$HOME/wavemodels/swash/bin >> ~/.bashrc
   source ~/.bashrc

options for configuring SWASH
-----------------------------

If desired, the build can be configured by passing one or more options below to ``make config``.

    ===================  ===================================================================
    ``fc=<compiler>``    the Fortran90 compiler to use [default is determined by ``CMake``]
    ``mpi=on``           enable build of SWASH with MPI [``off`` by default]
    ``prefix=<folder>``  set the installation folder [``$HOME/wavemodels/swash by`` default]
    ===================  ===================================================================

For example, the following command

.. code-block:: bash

   make config fc=ifx prefix=/usr/local/swash

will configure SWASH to be built using ``ifx`` and then install it at ``/usr/local/swash``.

building with MPI support
-------------------------

The SWASH source code also supports memory-distributed parallelism for high performance computing applications.
A message passing approach is employed based on the Message Passing Interface (MPI) standard that enables communication between independent processors.

The Intel Fortran Essentials package also contains the Intel MPI Library.
This can be checked with the following command:

.. code-block:: bash

   mpirun --version

We proceed to build SWASH. First, we configure SWASH to be built with support for MPI, as follows

.. code-block:: bash

   make config fc=mpiifx mpi=on

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
