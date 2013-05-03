OSDC Quickstart
===============
1. Apply and obtain an account via either the `OSDC application <http://www.opensciencedatacloud.org/apply>`_ or the `Bionimbus PDC application <http://bionimbus-pdc.opensciencedatacloud.org/apply>`_. If you are not sure, you likely want the OSDC application.

  * **Note**: The OSDC consoles uses federated login. If your organization is on the list of `InCommons members <https://incommon.org/federation/info/all-orgs.html>`_, please apply with that email. Otherwise, we also accept Gmail and Yahoo! email addresses for the public clouds. For Bionimbus PDC, you must have a `eRA commons <https://public.era.nih.gov/commons/>`_ username and the appropriate `dbGaP <http://www.ncbi.nlm.nih.gov/gap>`_ authorization.

2. Log into the `main OSDC console <http://www.opensciencedatacloud.org/console>`_ for all resources except the Bionimbus PDC which has its own separate `console <http://bionimbus-pdc.opensciencedatacloud.org>`_. (:doc:`Details <console>`)

3. From the console, upload or generate a key pair to use for cloud access (:doc:`Details <console>`)

4. From the console, launch a virtual machine (VM) (:doc:`Details <console>`)

5. From a terminal, or a program like PuTTY in Windows, access your virtual machine by first ssh'ing into the appropriate login node with the username provided to you and then ssh'ing to your VM. If you're having trouble, please see :doc:`ssh`. The login nodes and the user to login into your VM are:

  =============  ====================================== ==================
  Cloud          Login Node                             VM Username
  =============  ====================================== ==================
  OSDC Adler     login.opensciencedatacloud.org         root
  OSDC Sullivan  login2.opensciencedatacloud.org        ubuntu
  Bionimbus PDC  bionimbus-pdc.opensciencedatacloud.org same as login node
  Conte          conte.bionimbus.org         same as login node
  =============  ====================================== ==================

6. Compute over data!
