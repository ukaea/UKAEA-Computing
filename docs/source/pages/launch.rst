.. _launchis:

.. note:

        Make sure the module slurm is loaded: ``module load slurm``.

Launching jobs
==============

Using Interactive Session
-------------------------

You might launch jobs directly to see how they are performing. 

To start with, resources and allocations are requested with:

.. code-block:: bash

        sintr -A UKAEA-AP001-CPU -N15 -n840 -t 24:00:00 -p cclake

Breaking down the above sintaxis:

* `sintr` stands for *Session Interactive Session*.

* `-A` for account. For UKAEA, it is UKAEA-AP001-CPU.

* `-N` indicates the number of nodes, in this case 15.

* `-n` shows the total number of cores = 15 * 56 = 840.

* `-t` time is represented here with the format hh:mm:ss.

* `-p` the partition of the cluster used is *cclake*.

When the command prompt is popping up after a while, then

you will be able to enter the corresponding tasks to be launched:

.. code-block:: bash

        <myprogram> <myparameter1>  >>  <myoutput1>

An output is generated for parameter1. Then, you might be able to launch:

.. code-block:: bash

        <myprogram> <myparameter2>  >>  <myoutput2>

The output for parameter2 will be displayed and store in the same folder.


Simple Linux Utility for Resource Management (`SLURM <https://slurm.schedmd.com/sbatch.html>`_)
-----------------------------------------------------------------------------------------------

The procedure below (three steps) can be also launched by using a script:

.. code-block:: bash

        #!/bin/bash
        #SBATCH -J <myjobname>
        #SBATCH -A UKAEA-AP001-CPU
        #SBATCH -p cclake
        #SBATCH --nodes=15
        #SBATCH --ntasks=840
        #SBATCH --time=24:00:00

        <myprogram> <myparameter1>  >>  <myoutput1>
        <myprogram> <myparameter2>  >>  <myoutput2>

In this case, you wait once to have the outputs stored in the same folder.


SBATCH directives
-----------------

1. To submit a bash script with SLURM: ``sbatch <myscript>``.

2. To display all the scripts (jobs) a user submitted: ``squeue -u <myuser>``.

3. To cancel a specific job already launched: ``scancel <myjob>``.

4. To cancel all the jobs of a user: ``scancel --user=<myuser>``. 


Further options to be used for these directives: `sbatch <https://slurm.schedmd.com/sbatch.html>`_, `squeue <https://slurm.schedmd.com/squeue.html>`_, `scancel <https://slurm.schedmd.com/scancel.html>`_.

* Commonly, the **status** of the jobs shown in the system are:

*R (running), PD (pending), CG (completing), CD (completed), F (failed).*

Using Parallelisation
---------------------

* CSD3 usually allows performing until 36 hours per script.

* Make sure you load the appropriate modules to make a script works.

**OpenMP**

The use of 2 threads in only one node of the cluster to be run in 5 minutes:

.. code-block:: bash

        #!/bin/bash
        #SBATCH -J <my_openmp_job>
        #SBATCH -A UKAEA-AP001-CPU
        #SBATCH -p cclake
        #SBATCH --nodes=1
        #SBATCH --ntasks=1
        #SBATCH --cpus-per-task=2
        #SBATCH --time=00:05:00
        module purge
        module load slurm
        module load rhel7/global
        module load <mymodules>
        module list
        ulimit -n 65536
        export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
        <myapplication> -n $SLURM_NTASKS <myprogram> --n-threads=$OMP_NUM_THREADS <myinput> >> <myoutput>


**MPI**

The use of 112 MPI processes allocated in 2 nodes (56 processes per node): 

.. code-block:: bash

        #!/bin/bash
        #SBATCH -J <my_mpi_job>
        #SBATCH -A UKAEA-AP001-CPU
        #SBATCH -p cclake
        #SBATCH --nodes=2
        #SBATCH --ntasks=112
        #SBATCH --cpus-per-task=1
        #SBATCH --time=00:05:00
        module purge
        module load slurm
        module load rhel7/global
        module load <mymodules>
        module list
        ulimit -n 65536
        export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
        <myapplication> -n $SLURM_NTASKS <myprogram> --n-threads=$OMP_NUM_THREADS <myinput> >> <myoutput>

**Hybrid (OpenMP + MPI)**

The use of 2 MPI processes and 112 threads allocated in 2 cluster's nodes:

.. code-block:: bash

        #!/bin/bash
        #SBATCH -J <my_hybrid_job>
        #SBATCH -A UKAEA-AP001-CPU
        #SBATCH -p cclake
        #SBATCH --nodes=2
        #SBATCH --ntasks=2
        #SBATCH --cpus-per-task=56
        #SBATCH --time=35:59:59
        module purge
        module load slurm
        module load rhel7/global
        module load <mymodules>
        module list
        ulimit -n 65536
        export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
        <myapplication> -n $SLURM_NTASKS <myprogram> --n-threads=$OMP_NUM_THREADS <myinput> >> <myoutput>

