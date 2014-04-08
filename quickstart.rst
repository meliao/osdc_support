OSDC Quickstart
===============

OSDC Sullivan Quickstart
-------------------------
1. Apply for and obtain a resource allocation and an account via the `OSDC application <http://www.opensciencedatacloud.org/apply>`_.   Please allow 2-3 weeks for our allocation committee to review your application.

.. NOTE:: The OSDC console uses federated login. If your organization is on the list of `InCommons members <https://incommon.org/federation/info/all-orgs.html>`_, `UK Federation members <http://www.ukfederation.org.uk/content/Documents/MemberList>`_, or `CANARIE members <http://www.canarie.ca/en/about/partners/members>`_, please apply using those credentials.

2. Once your request has been approved and you receive a welcome email with details, log into the `main OSDC console <http://www.opensciencedatacloud.org/console>`_.

3. From the console, upload or generate a key pair to use for cloud access (:doc:`Details <console>`)

4. From the console, launch a virtual machine (VM) (:doc:`Details <console>`).   You can start with a plain vanilla image, or use a preexisting snapshot or image that has been already set up by your lab or another user with the required tools and software.  

.. Topic:: Coming Soon - Currently in BETA
	
		Updates to the Tukey Webconsole will allow OSDC users to more easily share descriptions of their image snapshots and the types of analysis the image is intended for.  

5. From a terminal, or a program like PuTTY in Windows, access your virtual machine by first ssh'ing into the appropriate login node with the username provided to you and then ssh'ing to your VM. If you're having trouble, please see :doc:`ssh`. The login nodes and the user to login into your VM are:

  ====================  ====================================== ==================
  Cloud                 Login Node                             VM Username
  ====================  ====================================== ==================
  OSDC Sullivan         sullivan.opensciencedatacloud.org      ubuntu
  ====================  ====================================== ==================

6. On Sullivan and the protected data clouds your user data is automatically mounted.  

7. Install any necessary software packages, etc necessary for your research.   

8. NEED A FUNCTIONAL EXAMPLE OF LOADING A DATASET FROM ROOT

9. NEED A FUNCTIONAL EXAMPLE OF DOING AN ANALYSIS ON THAT DATA USING A VM

10.  Remember to terminate your VMs in the Tukey console when not in use to free up valuable cores for other OSDC users.  Make sure if it's something you built from scratch, to snapshot it first!


Sullivan Quickstart - Terminal Commands
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Below is a screencapture of a terminal session showing the command line tools necessary to login to the OSDC Sullivan headnode and then a VM.  Feel free to copy and paste commands to your own shell, adjusting usernames and VM IPs as needed.

.. raw:: html

	<p><script type="text/javascript" src="https://asciinema.org/a/8758.js" id="asciicast-8758" async></script></p>

OSDC Atwood (Placeholder) Quickstart
-------------------------------------
1. Apply for and obtain a resource allocation and an account via the `OSDC application <http://www.opensciencedatacloud.org/apply>`_.   Please allow 2-3 weeks for our allocation committee to review your application.

  * **Note**: The OSDC consoles uses federated login. If your organization is on the list of `InCommons members <https://incommon.org/federation/info/all-orgs.html>`_, `UK Federation members <http://www.ukfederation.org.uk/content/Documents/MemberList>`_, or `CANARIE members <http://www.canarie.ca/en/about/partners/members>`_, please apply using those credentials.

2. Once your request has been approved and you receive a welcome email with details, log into the `main OSDC console <http://www.opensciencedatacloud.org/console>`_.

3. From the console, upload or generate a key pair to use for cloud access (:doc:`Details <console>`)

4. From the console, launch a virtual machine (VM) (:doc:`Details <console>`).   You can start with a plain vanilla image, or use a preexisting snapshot or image that has been already setup by your lab with the proper tools.  

  * **Note**: Coming soon we'll be expanding our Tukey console so that users can more easily share descriptions of their images and the types of work the image is intended for.  

5. From a terminal, or a program like PuTTY in Windows, access your virtual machine by first ssh'ing into the appropriate login node with the username provided to you and then ssh'ing to your VM. If you're having trouble, please see :doc:`ssh`. The login nodes and the user to login into your VM are:

  ====================  ====================================== ==================
  Cloud                 Login Node                             VM Username
  ====================  ====================================== ==================
  OSDC Adler            adler.opensciencedatacloud.org         root
  OSDC Sullivan         sullivan.opensciencedatacloud.org      ubuntu
  Bionimbus PDC         bionimbus-pdc.opensciencedatacloud.org same as login node
  OSDC Atwood (Conte)   atwood.opensciencedatacloud.org        same as login node
  OSDC Goldberg         goldberg.opensciencedatacloud.org      same as login node
  ====================  ====================================== ==================

6. On Atwood and the protected data clouds your user data is automatically mounted.

7. Install any necessary software packages, etc necessary for your research.  

8. Compute over data!

Atwood Quickstart - Terminal Commands
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Below is a screencapture of a terminal session showing the command line tools necessary to login to the OSDC Sullivan headnode and then a VM.  Feel free to copy and past commands to your own shell, adjusting usernames and VM IPs as needed.

.. raw:: html

	<p><script type="text/javascript" src="https://asciinema.org/a/8754.js" id="asciicast-8754" async></script></p>

OSDC Bionimbus-PDC (Placeholder) Quickstart
--------------------------------------------


Hadoop (OCC-Y and Skidmore) (Placeholder) Quickstart
----------------------------------------------------