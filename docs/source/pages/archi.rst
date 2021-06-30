.. _archis:

Architecture
============

Hardware
--------
The relevant cluster for UKAEA is the **Cascade Lake** cluster. 
Currently, this consists of **672 nodes**, each containing 2 sockets, each with 
Intel Xeon Platinum 8276 CPU @ 2.20GHz (i.e. 56 CPU cores in total
per node), **192 GB of memory**, Mellanox HDR100 interconnect.
 
The interconnect topology is 12 islands, each made up of 56 nodes
connected at HDR100 with full-bandwidth. The islands are connected
via fat tree with 3:1 oversubscription.
 
**UKAEA have a 112 node share** of this system.
 

Software
--------

The operating system is **Red Hat Enterprise Linux 7**. 

Modules
^^^^^^^

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
