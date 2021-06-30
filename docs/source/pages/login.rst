.. _logins:

Login to CSD3
=============

Generate the SSH Keys 
---------------------

* If you are using Windows, please follow this `procedure <https://techbast.com/2018/11/sophos-firewall-how-to-set-up-public-key-authentication-for-admin.html>`_.

For Linux users, open the terminal to do as follows:

1. Use ``ssh-keygen`` to create a RSA key pair.

2. Usually, it is saved on your local computer in ``$HOME/.ssh/id_rsa``.

3. Copy your RSA key pair by using the following command:
   
   ``ssh-copy-id <myusername>@login.hpc.cam.ac.uk``.

Connect to CSD3
---------------

* To connect to the cluster, you need to install first, the `SSH protocol <https://en.wikipedia.org/wiki/Secure_Shell_Protocol>`_.

        * If you are using Windows, you might follow these `steps <https://notesread.com/install-ssh-in-windows-10/>`_.

        * If you are using Linux, depends of the distribution, you might install it by:

                * sudo dnf install -y openssh-server

                * sudo apt-get install openssh-server

* Connect to the **CPU nodes** at CSD3: 
  
  ``ssh <myusername>@login.hpc.cam.ac.uk``.

* Connect to the **GPU nodes** at CSD3: 

  ``ssh <myusername>@login-gpu.hpc.cam.ac.uk``.
