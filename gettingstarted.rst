Getting Started with the Open Science Data Cloud
================================================

Requesting an OSCD Account
--------------------------
To request an account on the OSDC, please visit the `Account Application page  <https://www.opensciencedatacloud.org/apply/>`_ and provide the necessary information.

To apply for an account on the OSDC, you must provide an SSH public key. This key will allow you to connect to the login server through a Secure Shell (SSH). The SSH public key is one half of a SSH keypair, which is for secure interactions between two networked computers.


Key Generation on UNIX
~~~~~~~~~~~~~~~~~~~~~~
On a Unix machine (a machine running Linux, Mac OS X, etc.) open Terminal. Enter (with your own email address):

  ``ssh-keygen -t rsa -C "youremail@yourdomain.ext"\``

You will be prompted to enter a filename; you may press return for the default file name, or make up your own. You will then be prompted for a passphrase. After entering a passphrase for the key, your public-private keypair will have been generated. 

Now, you must copy the *public* key to a plain text file to upload on the Account Application page. Important note: you should **never** share your private key with another party, sharing is what the public key is for. If you are using Mac OS X, to copy your public key to your clipboard, enter:

  ``pbcopy < ~/.ssh/id_rsa.pub``
  
or on Linux:

  ``sudo apt-get install xclip``
  
  ``xclip -sel clip < ~/.ssh/id_rsa.pub``
  
in either case replacing ``id_rsa`` with the filename you selected above. Now, just create a text file, paste the key into the file, and upload that file on the Account Application page.


Key Generation on Windows
~~~~~~~~~~~~~~~~~~~~~~~~~
On a Windows machine, download and install `PuTTY <http://www.chiark.greenend.org.uk/~sgtatham/putty/>`_, a client program for the SSH protocol. Navigate to the Start menu, and under the PuTTY submenu open the PuTTYgen program. Press Generate. 

??????????????????????

OSDC Monthly Maintenance
------------------------
Monthly maintenance will take place on the last Tuesday of each month at 0900CT. Please make sure to save your work and shutdown all instances by that time. A reminder will be sent each month.


Connecting to the OSDC
----------------------
Connection to the OSDC can be done through two ways: connectiong through a Secure Shell (SSH), or connection through the Console.


Connecting through SSH
~~~~~~~~~~~~~~~~~~~~~~
To connect to the login server from a UNIX machine, open Terminal and enter:

  ``ssh -A username@login2.opensciencedatacloud.org``

Including ``-A`` is necessary because it will allow the server to forward your credentials to the virtual machine (VM). Otherwise, you will get an error when you attempt to connect to the VM. You will be prompted for a passphrase; this will be the passphrase you selected when generating the key. Enter it. 

Connection through the Console
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
To connect to the OCDC through the `Console <www.opensciencedatacloud.org/console/>'_, 


Workflow
--------
ssh to the login server

start a VM

ssh to VM

mount shared filesystem with own credentials

…compute!…

when all done, stop your VM


Open Science Data Cloud basic usage
-----------------------------------
In your home directory on the login server (login1.opensciencedatacloud.org), there is a .euca subdirectory containing your EC2 (eucalyptus) credentials. This directory has mode 700 as it contains cryptographic keys for accessing EC2. Keep it protected.

To list images, run:

  ``euca-describe-images``

Look for entries with an ID starting with ‘emi-*’ or ‘ami-*’. OSDC-provided prototypical images are owned by admin and start with ‘prototypical-’.

Following types of VMs are supported:

+------------+----------+------------+-------------+
| VM Type    | Cores    | RAM (MB)   | Disk (GB)   |
|            |          |            |             |
+============+==========+============+=============+
| m1.small   | 1        | 3584       | 20          |
+------------+----------+------------+-------------+
| m1.large   | 2        | 716        | 20          |
+------------+----------+------------+-------------+
| c1.medium  | 4        | 14336      | 20          |
+------------+----------+------------+-------------+
| m1.xlarge  | 8        | 28672      | 20          |
+------------+----------+------------+-------------+
| c1.xlarge  | 8        | 28672      | 20          |
+------------+----------+------------+-------------+

To check the cloud availability/usage on the Adler cloud:

  ``euca-cloud-status``

To start an instance on Adler run:

  ``euca-run-instances -k USERNAME -n 1 -t m1.small emi-ID``

To start an instance on Sullivan run:

  ``euca-run-instances -k USERNAME -n 1 -t m1.small -f ~/.smbpasswd.sh emi-ID``

This will start an m1.small instance as defined by the command above. Note that it is best to start one instance at a time. That is, if one needs 20 instances, they should be started one-at-a-time followed by a delay and then the next instance, until all 20 are running. This effectively resolves Eucalyptus software scalability issues.

To monitor/list instances of one’s group (only!), run:

  ``euca-describe-instances``

This will return status of instances, with instance ID and an instance IP address.

To login to an instance on Adler run:

  ``ssh root@INSTANCE_IP``

To login to an instance on Sullivan run:

  ``ssh ubuntu@INSTANCE_IP``

To mount shared filesystem on Adler instances, run following command upon logging in:

  ``/cloudconf/mount-glusterfs USERNAME``

and enter your password.

For Sullivan instances the option -f ~/smbpasswd.sh ensures the shared filesystem is automatically mounted.

To check output log of an instance, run:

  ``euca-get-console-output INSTANCE_ID``

To stop an instance, log out of the instance and run this command:

  ``euca-terminate-instances INSTANCE_ID``