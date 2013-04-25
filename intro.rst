Introduction to the Open Science Data Cloud
===========================================
.. figure:: _static/OSDCinfrastructure.png
   :width: 300
   :height: 222
   :alt: "Big-Picture" Diagram of OSDC Infrastructure
   :align: right

   Overview of OSDC Public Infrastructure

The `Open Science Data Cloud <https://opensciencedatacloud.org>`_
(OSDC) is a distributed, cloud-based infrastructure for managing,
analyzing, archiving and sharing scientific datasets. 

The OSDC operates two major public compute clouds named Adler and Sullivan and one storage cloud named Root. Adler is a `Eucalyptus <http://www.eucalyptus.com/>`_ based cloud and is also the host of Bionimbus Community Cloud. Sullivan is a newer `OpenStack <http://www.openstack.org/>`_ based cloud and the default cloud new users are placed on. Root hosts about 1 PB of `public datasets <http://www.opensciencedatacloud.org/publicdata>`_ and is accessible from all of the OSDC clouds.

The OSDC also operates several protected clouds to provide platforms for users to compute over large protected human genomic datasets. These include the `Bionimbus Protected Data Cloud <https://bionimbus-pdc.opensciencedatacloud.org>`_ and the `Bionimbus Conte Cloud <http://www.contechicago.org/conte-cores/core-b>`_.

The OSDC manages two Hadoop clusters, OCC-Y and LVOC, for select projects. The current documentation here does if you are interested in these please contact us at info@opencloudconsortium.org.


OSDC Quickstart
--------------------------------------------
1. Apply and obtain an account via either the `public resource application <http://www.opensciencedatacloud.org/apply>`_ or the `protected resource application <http://bionimbus-pdb.opensciencedatacloud.org/apply>`_. If you are not sure, use the public resource application.

  * **Note**: The OSDC consoles uses federated login. If your organization is on the list of `InCommons members <https://incommon.org/federation/info/all-orgs.html>`_, please apply with that email. Otherwise, we also accept gmail and yahoo email addresses.

2. Log into the `main OSDC console <http://www.opensciencedatacloud.org/console>`_ for all resources except the Bionimbus Protected Data Cloud, which has its own separate `console <http://bionimbus-pdc.opensciencedatacloud.org>`_. (details ref)

3. From the console, upload or generate a key pair to use for cloud access (details ref)

4. From the console, launch a virtual machine (VM) (details ref)

5. From a terminal, or a program like PuTTY in Windows, access your virtual machine by first ssh'ing into the appropriate login node and then ssh'ing to your VM. Please see (page here) if you have trouble with this step. The login nodes are:

  * **OSDC Adler**: login.opensciencedatacloud.org
  * **OSDC Sullivan**: login2.opensciencedatacloud.org
  * **Bionimbus PDC**: bionimbus-pdc.opensciencedatacloud.org
  * **Conte**: conte.bionimbus.org

6. Compute over data!


