Using Tukey, The OSDC Console
=============================
Once you have an `OSDC account  <https://www.opensciencedatacloud.org/apply/>`_ you can manage your cloud resources through the `OSDC Console <https://www.opensciencedatacloud.org/consle/>`_ .


Login
-----
The OSDC console authenticates using Single Sign-On.  We recommend users login using The `InCommon Federation <https://incommon.org/federation/>`_.  If you do not have credentials from a `participating organization <https://incommon.org/federation/info/all-entities.html>`_ you can use Google or Yahoo!

To sign in select the organization of the "Login E-mail" you provided in your application.  For example if I entered mgreenway@uchicago.edu in my application I would select "University of Chicago".  The console will then redirect to your identity provider's login page.  Once you have authenticated your identity provider will redirect you back to the OSDC Console.


Managing SSH Keys
-----------------
Users access OSDC login nodes and virtual machine instances with SSH keypair authentication.  Before launching a virtual machine you must import a public key or create a new keypair.  To mange your keypairs go to `Access & Security <https://www.opensciencedatacloud.org/project/access_and_security/>`_.


Importing Keys
~~~~~~~~~~~~~~
To import a public key click "Import Keypair" name the keypair then paste the OpenSSH format text of the public key into the Public Key field.  The public key text must all be on one line.

Creating Keypairs
~~~~~~~~~~~~~~~~~
To create a keypair click "Create Keypair" name the keypair then

