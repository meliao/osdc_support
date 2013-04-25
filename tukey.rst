Using Tukey, The OSDC Console
=============================
Once you have an `OSDC account  <https://www.opensciencedatacloud.org/apply/>`_ you can manage your cloud resources through the `OSDC Console <https://www.opensciencedatacloud.org/consle/>`_ .


Login
-----
Users authenticate to the OSDC console using Single Sign-On.  We recommend users login using The `InCommon Federation <https://incommon.org/federation/>`_.  If you do not have credentials from a `participating organization <https://incommon.org/federation/info/all-entities.html>`_ you can use Google or Yahoo!

To sign in select the organization of the "Login E-mail" you provided in your application.  For example if I entered mgreenway@uchicago.edu in my application I would select "University of Chicago".  The console will then redirect to your identity provider's login page.  Once you have authenticated your identity provider will redirect you back to the OSDC Console.


Managing SSH Keys
-----------------
Users access OSDC login nodes and virtual machines through SSH using keypair authentication.  Before launching a virtual machine you must import a public key or create a new keypair.  To mange your keypairs go to `Access & Security <https://www.opensciencedatacloud.org/project/access_and_security/>`_.

Creating Keypairs
~~~~~~~~~~~~~~~~~
To create a keypair click "Create Keypair" name the keypair then select the resource.  Click "Create Keypair" then Download keypair on the download page.

Example:
^^^^^^^^
* Click "Create Keypair"
* In "Keypair Name" enter "osdc_keypair"
* Under "Resources" select "All Resources"
* Click "Create Keypair"

.. figure:: _static/create_keypair.png

* Launch and instance and select the keypair "osdc_keypair.
* If you are using OpenSSH (see :doc:`/ssh_troubleshooting` for other SSH clients) ``ssh-add osdc_keypair.pem``
* ``ssh -A -i osdc_keypair.pem login2.opensciencedatacloud.org`` to access the login node.
* SSH to your vm ``ssh ubuntu@INSTANCE_IP``.  If you are not launching instances on the Sullivan cloud replace login2.opensciencedatacloud.org with the hostname of that cloud and replace ubuntu as shown in :doc:`/quickstart`


Importing Keys
~~~~~~~~~~~~~~
To import a public key click "Import Keypair" name the keypair then paste the OpenSSH format text of the public key into the Public Key field.  The public key text must all be on one line.  Then select the resource you will use this keypair to access.


Differences Between Protected Clouds and Public Clouds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
On public clouds such as Adler and Sullivan you must have keypair for your virtual machine instances.  On protected clouds your home directory is shared between the login nodes and the virtual machines.  On both public and protect clouds you will need a keypair for the login server.  On protected clouds you can use this same keypair to access VMs using SSH key forwarding.
