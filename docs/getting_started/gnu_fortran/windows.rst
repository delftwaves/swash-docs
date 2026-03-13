Windows
=======

prerequisites
-------------

The following packages must be installed first:

- gfortran
- gmake
- cmake
- ninja
- perl

These packages are all included in the `Strawberry Perl <https://strawberryperl.com>`_ package.

After installing the latest release of Strawberry Perl, check whether the required packages were successfully installed on your computer,
by opening a command prompt (press Windows key + R, type ``cmd``, and hit Enter) and running the following commands::

   gfortran --version

   cmake --version

   ninja --version

   perl --version

If each of the above package reports a version number, then the installation was successful.

.. important::

   - The version number of ``CMake`` must be at least 3.20 or newer.
   - The ``perl`` version is 5 or higher.

installation SWASH
------------------

Once the prerequisites are taken care of, installing SWASH on your machine is a four-step process.

1. download SWASH

   Open a command prompt, copy the command below, right-click in the prompt window to paste and press Enter.

   .. code-block:: text

      git clone https://gitlab.tudelft.nl/citg/wavemodels/swash.git && cd swash

2. configure SWASH

   .. code-block:: text

      gmake config

3. build SWASH

   .. code-block:: text

      gmake

4. install SWASH

   .. code-block:: text

      gmake install

SWASH is installed at folder ``%LocalAppData%\Programs\wavemodels\swash`` by default. The current value of ``%LocalAppData%`` can be checked by typing

.. code-block:: text

   echo %LocalAppData%

To run SWASH, you need to make sure that the ``\bin`` folder in this directory is added to your system's ``PATH``. Open command prompt and type

.. code-block:: text

   setx path "%path%;%LocalAppData%\Programs\wavemodels\swash\bin"

.. warning::

   To check the new value of ``%path%`` by echoing, first close the command prompt terminal and then open again.

options for configuring SWASH
-----------------------------

If desired, the build can be configured by passing the option ``prefix=<folder>`` to ``gmake config``.
For example, the following command

.. code-block:: text

   gmake config prefix=C:\Program Files\swash

will configure SWASH to be installed at ``C:\Program Files\swash``.

.. note::

   Unfortunately, there is no simple way to build SWAN with MPI support using gfortran.
   One consideration is to build using :ref:`Intel Fortran + MPI <intelwin>`.
   Another option is to build within the :ref:`Docker container <indoc>`.

clean up
--------

To remove all files installed by ``gmake install``, type the following command

.. code-block:: text

   gmake uninstall

If you want to remove the build directory and all files that have been created after running ``gmake`` or ``gmake build``, then run

.. code-block:: text

   gmake clobber
