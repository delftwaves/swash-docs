from Docker image
=================

The user can either choose between running SWASH directly using docker or the docker in interactive mode (flag ``-it``).

To run SWASH directly, copy and paste the following command, replace the required run parameters, and hit Enter::

   docker run --rm -v .:/home/swash delftwaves/swash swashrun -input <SWASH-command-file-without-extension> -mpi <nprocs>

To run the SWASH container interactively, enter::

   docker run --rm -v .:/home/swash -it delftwaves/swash bash

Once the interactive bash shell is started in the container, the user can access the commands available from Linux Ubuntu (e.g., ``ls``, ``find``,
``which``) and SWASH (e.g., ``swashrun``, ``swash.exe``).

.. note::

   - The directive ``-v .:/home/swash`` ensures that the SWASH output files and the PRINT file created in the directory ``/home/swash`` of the
     docker container will store in your local current directory.
   - The flag ``--rm`` removes the exited container from your machine after terminating SWASH. (Check by invoking the command ``docker ps -a``.)
   - For a complete overview of the command ``docker run``, please refer to this `page <https://docs.docker.com/reference/cli/docker/container/run/>`_.
