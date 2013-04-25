Trouble Shooting SSH connections
================================

Can not ssh into the Login nodes?
---------------------------------
    #. Have you Uploaded your Key via tukey :doc:`/tukey` ?
    #. Have you Configured your private key in :ref:`ssh-agent <ssh-key-forwarding>` ?
   
Can you not login to your VM's once created.
--------------------------------------------
    #. :ref:`Ensure that you have enabled ssh key forwarding <ssh-key-forwarding>` ?
    #. :ref:`Is the ssh key showing up as forwarded on the login node <display-agent-keys>`?


Common SSH tasks
================

.. _ssh-key-forwarding:

Loading your private key into an SSH agent
------------------------------------------

.. _ssh-agent:

LINUX/OSX
^^^^^^^^^
a
.. _pagent.exe:

Windows via Putty/Pagent.exe
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
a

.. _display-agent-keys:

Showing keys loaded into your agent on Linux/OSX/etc
----------------------------------------------------
Running the ``ssh-add -l``  command will display all keys currently loaded into your ssh agent.  Run this command from a shell (if not using putty) before ssh'ing into the login node to confirm that your key is properly loaded. Run it again once you have ssh'ed into the login node to confirm the key has properly forwarded.  If you do not see the key showing up on the login node, you will not be able to access your started Virtual Machines.

Example Output: 
.. code-block::
    $ ssh-add -l 
    1024 1a:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff:f1 /Users/JohnSmith/.ssh/id_dsa (DSA)`

