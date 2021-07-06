.. _troublis:

On CSD3
=======

The log of how the job is performing is stored in the **slurm-<myjobID>** file.
You might see the full message to realize what is going on. Some cases:

* The need of memory is expressed when you have a message like this: 

.. code-block:: bash

   mpirun noticed that process rank 53 with PID 0 on node cpu-p-435 exited on signal 9 (Killed).

* The program is invoking the MPI_Abort() routine and stop it:

.. code-block:: bash

   MPI_ABORT was invoked on rank 0 in communicator MPI_COMM_WORLD
   with errorcode 1.

   NOTE: invoking MPI_ABORT causes Open MPI to kill all MPI processes.
   You may or may not see output from other processes, depending on
   exactly when Open MPI kills them.

* "The Slurm job cleanup stage runs after your job script completes." 

.. code-block:: bash 

   slurmstepd: error: task_p_post_term: rmdir(/sys/fs/cgroup/cpuset/slurm42221993/slurm42221993.4294967294_0) 
   failed Device or resource busy

Support
=======

* Contact support@hpc.cam.ac.uk with the Subject **UKAEA - <myissue>** in case of issues.
