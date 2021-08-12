.. _appis:

MOOSE
=====

MOOSE computationally solves PDEs by configuring the mesh of the domain, boundary conditions, variables, kernels and preconditioners in the *input file*.

Basically, there are two main steps to perfom them, one is the splitting part, and the other is, the execution part to solve the PDE by using the Finite Element Method.

Due to the amount of data performed for discretisation, it is recommendable to create first, the mesh, before running the execution. 

* *An MPI line command to split the mesh in MOOSE*

.. code-block:: bash
 
        mpirun -n 8 -npernode 56 /home/ir-inca1/projects/pooka/pooka-opt  -i input.i  --split-mesh 112,224,336,448 --split-file hpcmesh5120.cpr >> ja_th_NE_mpi5120.out -log_view


* *An MPI script example to run input file of MOOSE ten times*

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

.. warning::

        * Make sure of the spelling of the names and location of the folders.

        * Try to not use more than eith caracters for the jobname. 

        * Load the corresponding modules in the script to make MOOSE works.

        * Configure the environment and the resource limit in the script.


NekRS-and-Nek5000
=================

The following configuration was done using the GPU nodes in CDS3: `ssh <my_user>@login-gpu.hpc.cam.ac.uk`

.. code-block:: bash

   module purge
   module load dot
   module load slurm
   module load rhel7/global
   module load cmake/latest
   module load rhel7/default-peta4
   module load git-2.31.0-gcc-5.4.0-ec3ji34
   module load python/3.8
   module load gcc/7
   module load openmpi-3.1.3-gcc-7.2.0-b5ihosk
   module load cuda/9.2
   module load nsight/systems-2020.3.1
   export MPI_MCA_mca_base_component_show_load_errors=0
   export PMIX_MCA_mca_base_component_show_load_errors=0
   ulimit -n 65536
   module list
