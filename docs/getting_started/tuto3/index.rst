.. _tuto3:

tutorial 3: modify input files
==============================

In this tutorial, we will take a closer look at the command file and subsequently make some changes.

The aim now is to simulate shorter waves propagating on a sloping beach.
For example, we want to reduce the peak period of incident waves from 10 s to 5 s.
At the offshore location with a depth of 10 m the :math:`kd` value will increase from 0.68 to 1.7 (:math:`k` is the wave number and :math:`d` is the still water depth).
In practice, this means using two layers instead of one.
However, this will increase the run time. To make it manageable for this tutorial, we will reduce the simulation time by a factor of two.

The bed profile remains unchanged.

step 1: change boundary condition
---------------------------------

First, navigate to your working directory and then edit the file ``bc.txt``. Modify this file as follows:

#. Replace 10 s by 5 s.
#. Shorten the cycle period from 20 min to 10 min.

.. note::

   The reason for shortening the cycle period is related to shortening the simulation period.

step 2: modify command file
---------------------------

Now it's time to edit the command file. First, make a copy of ``wavbrk.sws``. (This file was created in the previous :ref:`tutorial <tuto2>`.)

The following components need to be modified or added:

#. Specify two layers using the command ``VERT``. Click
   `here <https://swash.sourceforge.io/online_doc/swashuse/swashuse.html#dx1-32005>`_ for a more precise specification.
#. Shorten the duration for computing the significant wave height from 20 min to 10 min (see command ``QUANT``).
#. Shorten the simulation time from 30 min to 15 min (see command ``COMPUTE``).

.. hint::

   Determine where to place the command ``VERT 2`` in the command file by consulting this
   `page <https://swash.sourceforge.io/online_doc/swashuse/swashuse.html#sequence-of-commands>`_.

step 3: run new simulation
--------------------------

From now on we will be working with the official `Docker image <https://hub.docker.com/r/delftwaves/swash>`_ for running SWASH simulations.
Let us pull this image first:

.. code-block:: bash

   docker pull delftwaves/swash

Run your simulation by entering the following command:

.. code-block:: bash

   docker run -v .:/home/swash delftwaves/swash swashrun -input <your-command-file>

with ``your-command-file`` your modified command file based on the previous step.

.. tip::

   You can also give a shorter name for the image here:

   .. code-block:: bash

      docker tag delftwaves/swash <your-short-name>

step 4: post processing
-----------------------

Once the run is complete, it's time to examine the model results using the plot scripts as explained in this :ref:`step <step6>`.

Are the new results correct? Can you explain, for example, why these results differ from previous simulations?
