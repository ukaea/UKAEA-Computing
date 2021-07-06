.. _csd3is:

CSD3
====

Access
------
* Fill in the Hardware access request `form <https://forms.office.com/Pages/ResponsePage.aspx?id=S2asxieuXU205rtXFxlvxwkLUZWKc5VGpUXyGnokgFFUNlI1UVIyVVk3RUZOSUpKVjUxVldCNzJZRi4u>`_.
* Get a SAFE Account Request an account `here <https://dirac-safe.readthedocs.io/en/latest/safe-guide-users.html#how-to-request-a-dirac-system-account>`_.
* Request to join the **UKAEA project - ap001**.
* | This will ultimately go to `Shaun De Witt <shaun.de-witt@ukaea.uk>`_, `Andrew Davis <andrew.davis@ukaea.uk>`_,
  | `Andrew Lahiff <andrew.lahiff@ukaea.uk>`_ or `Aleks Dubas <aleksander.dubas@ukaea.uk>`_ for approval.


Architecture
------------

Hardware
^^^^^^^^

The relevant cluster for UKAEA is the **Cascade Lake** cluster.
Currently, this consists of **672 nodes**, each containing 2 sockets, each with
Intel Xeon Platinum 8276 CPU @ 2.20GHz (i.e. 56 CPU cores in total
per node), **192 GB of memory**, Mellanox HDR100 interconnect.

The interconnect topology is 12 islands, each made up of 56 nodes
connected at HDR100 with full-bandwidth. The islands are connected
via fat tree with 3:1 oversubscription.

**UKAEA have a 112 node share** of this system.


Software
^^^^^^^^

The operating system is **Red Hat Enterprise Linux 7**.

**Modules**

There is a collection of software packages and libraries already built
on the cluster to be loaded and used. This collection is called `module`.

*Some useful* **commands** *to handle modules as follows:*

1- To find out what sofware is already loaded in the cluster:

.. code-block:: bash

   module list

2- In case you do not need any of those software already listed:

.. code-block:: bash

   module purge

3- To see what are the modules available to be used in the cluster:

.. code-block:: bash

   module avail

3- To find out what software you specifically need to install:

.. code-block:: bash

   module avail | grep <mysoftware>

4- In case you need the description of the module found:

.. code-block:: bash

   module whatis <mysoftware>

5- Further information of the module found such as dependencies:

.. code-block:: bash

   module show <mysoftware>

6- To see the different versions of the software package:

.. code-block:: bash

   module apropos <mysoftware>

7- If you have found what are the software you need to use:

.. code-block:: bash

   module load <mysoftware1>
   module load <mysoftware2>
   ...

.. note::

   Remember to check out what are the current modules loaded,
   by using the command `module list`. Use *module help* for further help.


Login to CSD3
-------------

**Generate the SSH Keys** 

* If you are using Windows, please follow this `procedure <https://techbast.com/2018/11/sophos-firewall-how-to-set-up-public-key-authentication-for-admin.html>`_.

For Linux users, open the terminal to do as follows:

1. Use ``ssh-keygen`` to create a RSA key pair.

2. Usually, it is saved on your local computer in ``$HOME/.ssh/id_rsa``.

3. Copy your RSA key pair by using the following command:
   
   ``ssh-copy-id <myusername>@login.hpc.cam.ac.uk``.

**Connect to CSD3**

* To connect to the cluster, you need to install first, the `SSH protocol <https://en.wikipedia.org/wiki/Secure_Shell_Protocol>`_.

        * If you are using Windows, you might follow these `steps <https://notesread.com/install-ssh-in-windows-10/>`_.

        * If you are using Linux, depends of the distribution, you might install it by:

                * sudo dnf install -y openssh-server

                * sudo apt-get install openssh-server

* Connect to the **CPU nodes** at CSD3: 
  
  ``ssh <myusername>@login.hpc.cam.ac.uk``.

* Connect to the **GPU nodes** at CSD3: 

  ``ssh <myusername>@login-gpu.hpc.cam.ac.uk``.


Launching jobs
--------------

.. note:

        Make sure the module slurm is loaded: ``module load slurm``.

**Using Interactive Session**

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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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


*SBATCH directives*

1. To submit a bash script with SLURM: ``sbatch <myscript>``.

2. To display all the scripts (jobs) a user submitted: ``squeue -u <myuser>``.

3. To cancel a specific job already launched: ``scancel <myjob>``.

4. To cancel all the jobs of a user: ``scancel --user=<myuser>``. 


Further options to be used for these directives: `sbatch <https://slurm.schedmd.com/sbatch.html>`_, `squeue <https://slurm.schedmd.com/squeue.html>`_, `scancel <https://slurm.schedmd.com/scancel.html>`_.

* Commonly, the **status** of the jobs shown in the system are:

*R (running), PD (pending), CG (completing), CD (completed), F (failed).*


**Using Parallelisation**

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

