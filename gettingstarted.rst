Getting Started with the Open Science Data Cloud
================================================



Who should apply for an OSDC Account?
-------------------------------------
The OSDC is intended to provide resources for scientists and researchers that are working on data-intensive projects...



Requesting an OSCD Account
--------------------------
To request an account on the OSDC, please visit the `Account Application page  <https://www.opensciencedatacloud.org/apply/>`_ and provide the necessary information.


Key Generation
~~~~~~~~~~~~~~
To apply for an account on the OSDC, you must provide an SSH public key. This key will allow you to connect to the login server through a Secure Shell (SSH). The SSH public key is one half of a SSH keypair, which is for secure interactions between two networked computers.

On UNIX
^^^^^^^
On a UNIX machine (a machine running Linux, Mac OS X, etc.) open Terminal. Enter (with your own email address):

  ``ssh-keygen -t rsa -C "youremail@yourdomain.ext"\``

You will be prompted to enter a filename; if the default presented is ``id_rsa``, just press enter to accept. If the default presented is not that name, it may be because another file by that name already exists. Check the the ~/.ssh directory, and if this is the case, you should back up the existing ``id_rsa`` file. After checking and backing up if the file already existed, enter ``id_rsa`` for the filename of the new keypair you are generating. You will then be prompted for a passphrase. After entering a passphrase for the key, your public-private keypair will have been generated. 

Now, you must copy the *public* key to a plain text file to upload on the Account Application page. It is important to note that you should **never** share your private key with another party; sharing is what the public key is for. Sharing your private key could allow others to maliciously access systems that you trust. If you are using Mac OS X, to copy your public key to your clipboard, enter:

  ``pbcopy < ~/.ssh/id_rsa.pub``
  
Or on Linux:

  ``sudo apt-get install xclip``
  
  ``xclip -sel clip < ~/.ssh/id_rsa.pub``
  
Now, just create a text file, paste the key into the file, and upload that file on the Account Application page.

On Windows
^^^^^^^^^^
On a Windows machine, download and install `PuTTY <http://www.chiark.greenend.org.uk/~sgtatham/putty/>`_, an SSH protocol client program for Windows. Navigate to the Start menu, and under the PuTTY submenu open the PuTTYgen program. Press Generate
..more to be added here



OSDC Monthly Maintenance
------------------------
Monthly maintenance will take place on the last Tuesday of each month at 0900CT. Please make sure to save your work and shutdown all instances by that time. A reminder will be sent each month.



Connecting to the OSDC
----------------------
Connection to the OSDC can be done through two ways: connection through a Secure Shell (SSH), or connection through the Console. 


Connecting through SSH
~~~~~~~~~~~~~~~~~~~~~~
Secure Shell (SSH) is a network protocol that allows for secure communication between two networked computers over an unsecured network (e.g., the Internet). It is the protocol that is used to connect to the OSDC through a command line interaface. The method for connecting depends on the operating system of the machine you are using.

SSH on a UNIX Machine
^^^^^^^^^^^^^^^^^^^^^
To connect to the login server from a UNIX machine, open the Terminal and enter:

  ``ssh -A username@login2.opensciencedatacloud.org``

Including ``-A`` allows for `agent forwarding <http://www.unixwiz.net/techtips/ssh-agent-forwarding.html>`_; it is necessary because it will allow the server to forward your credentials to the virtual machine (VM). If it is not included, you will get an error when you attempt to connect to the VM. You will be prompted for a passphrase; this will be the passphrase you selected when generating the key. Enter it. 

SSH on Windows
^^^^^^^^^^^^^^
To connect to the login server from a Windows machine, 


Connection through the Console
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The OCDC allows two options for login through the `Console <www.opensciencedatacloud.org/console/>`_; Shibboleth and OpenID. If you have an account with a college or university, select ``Shibboleth`` and then select your institution. Enter and submit your credentials associated with that account (*not* your OSDC account). If you don't have an account with a college or university, enter your OpenID credentials to gaina ccess to the Console. 

After logging in, it is necessary to establish an SSH keypair for your work on the OSDC. Navigate to the "Access & Control" page. Now, select "Import Keypair." Enter a name for the keypair (traditionally firstinitiallastname), and then copy and paste your public key as described above (*hyperlink). Select "All resources" from the drop-down menu, and press submit. This will have established a keypair for use by the OSDC. 



Workflow
--------
ssh to the login server

start a VM

ssh to VM

mount shared filesystem with own credentials

…compute!…

when all done, stop your VM
