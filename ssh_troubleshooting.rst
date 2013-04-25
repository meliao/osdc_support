Trouble Shooting SSH connections
================================

Can not ssh into the Login nodes?
---------------------------------
    #. Have you Uploaded your Key via tukey :doc:`/console` ?
    #. Have you loaded your private key in :ref:`a ssh agent <ssh-key-forwarding>` ?

Can you not login to your VM's once created.
--------------------------------------------
    #. :ref:`Ensure that you have enabled ssh key forwarding <ssh-key-forwarding>` ?
    #. :ref:`Is the ssh key showing up as forwarded on the login node <display-agent-keys>`?
    #. :ref:`Are you enabling SSH key forwarding when ssh'ing into the login node. <ssh-A>`?


Common SSH tasks
================

.. _ssh-key-forwarding:

Loading your private key into an SSH agent
------------------------------------------

.. _ssh-agent:

LINUX/OSX
^^^^^^^^^
To load your key into the key agent simply run ``ssh-add ~/.ssh/keyname``. If you password protected your private key you will be asked to enter the password.   By default keyname will be either ``~/.ssh/id_dsa`` or ``~/.ssh/id_rsa``.  You will most likely need to run this  command each time you start a terminal/cli session.
If you are on OSX you can add this key to your OSX key ring by running ``ssh-add -k``. If you password protected your private key you will be asked this once for the password.  Everytime you open a new terminal window, OSX will auto populate the ssh keys you saved via ``ssh-add -k``.

Script to Auto start the ssh agent
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you are using a Modern Desktop Environment (KDE/GNOME) then it most likely is handling your ssh-agent/keyring.  However if yours does not, or your ssh key exists on an another server you can use the below script in your ``.bashrc`` file to auto start the agent.  Once started you still need to :ref:`add your keys <ssh-agent>`.  If you are not using bash as your shell then you will need to modify as needed to fit your prefered enviroment.  You may also wish to try `keychain <http://www.funtoo.org/wiki/Keychain>`_.

.. code-block:: bash

    SSH_ENV="$HOME/.ssh/environment"
    alias ssh="ssh -A"

    function start_agent {
        echo "Initialising new SSH agent..."
        /usr/bin/ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
        echo succeeded
        chmod 600 "${SSH_ENV}"
        . "${SSH_ENV}" > /dev/null
        /usr/bin/ssh-add;
    }
    # Source SSH settings, if applicable
    if [ -f "${SSH_ENV}" ]; then
         . "${SSH_ENV}" > /dev/null
         #ps ${SSH_AGENT_PID} doesn't work under cywgin
         ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || start_agent;
    else
         start_agent;
    fi

.. _pagent.exe:

Windows via Putty/Pagent.exe
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
INSERT WINDOWS INSTRUCTIONS and PICTURES

.. _display-agent-keys:

Showing keys loaded into your agent on Linux/OSX/etc
----------------------------------------------------
Running the ``ssh-add -l``  command will display all keys currently loaded into your ssh agent.  Run this command from a shell (if not using putty) before ssh'ing into the login node to confirm that your key is properly loaded. Run it again once you have ssh'ed into the login node to confirm the key has properly forwarded.  If you do not see the key showing up on the login node, you will not be able to access your started Virtual Machines.

Example Output:
.. code-block:: bash

    $ ssh-add -l
    1024 1a:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff:f1 /Users/JohnSmith/.ssh/id_dsa (DSA)`

.. _ssh-A:

Enabling SSH key forwarding
---------------------------
Once your agent is configured you need to enable forwarding.  You can use any one of the below methods.

* Open the ``ssh_config`` file located globally at ``/etc/ssh/ssh_config`` or locally at ``~/.ssh/config``. If this file does not exist under ``~/.ssh/`` then create it.  Add the following line ``ForwardAgent yes`` to this file.  All new connections will use forwarding.
* When ssh'ing to the login node, use the ``-A`` flag.  This turns on forwarding on a case by case basis.  IF you have multiple login nodes that you are transversing, you will need to use the ``-A`` flag for all hops.  Example: ``ssh -A JohnSmith@sullivan.opensciencedatacloud.org``
* Alias ``ssh -A`` as ``ssh`` via your shells prefered method.  On bash you can ``ALIAS ssh='ssh -A'``.


