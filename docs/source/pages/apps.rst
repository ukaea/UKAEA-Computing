.. _appis:

Applications
============

MOOSE
-----

* Make sure of the spelling of the names and location of the folders.

* Try to not use more than eith caracters for the jobname. 

* Load the corresponding modules in the script to make MOOSE works.

* Configure the environment and the resource limit in the script.

*An MPI example script to run input file of MOOSE ten times*

.. code-block:: bash

        #!/bin/bash
        #SBATCH -J hy_fl_NE
        #SBATCH -A UKAEA-AP001-CPU
        #SBATCH -p cclake
        #SBATCH --nodes=2
        #SBATCH --ntasks=112
        #SBATCH --cpus-per-task=1
        #SBATCH --time=06:00:00
        module purge
        module load slurm
        module load rhel7/global
        module load gcc/7
        module load openmpi-3.1.3-gcc-7.2.0-b5ihosk
        module load python/3.8
        module list
        ulimit -n 65536
        source ~/.moose_profile
        export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

        for i in {0..9}; do
        	mpirun -n $SLURM_NTASKS /home/ir-inca1/projects/pooka/pooka-opt --n-threads=$OMP_NUM_THREADS -i /rds/project/iris_vol2/rds-ukaea-ap001/prec_study/inputs/hypre/fluid3D/NEWTON/4/input.i >> hy_fl_NE_$i.out -log_view
        done

The simulation of multiphysics phenomena in MOOSE are too dense. 

It is recommendable to create the mesh first before run execution.

*An MPI example script to create a mesh file to be later use for calculations:*

.. code-block:: bash

   mpirun -n 8 -npernode 56 /home/ir-inca1/projects/pooka/pooka-opt  -i input.i  --split-mesh 112,224,336,448 --split-file hpcmesh10240.cpr >> mg_th_NE_mpi10240.out -log_view

