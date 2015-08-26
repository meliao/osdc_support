Griffin Resource Guide 
============================

.. _griffin:

.. sidebar:: Storage types - Ephemeral vs. Persistent
	
		**Ephemeral**
		"Ephemeral storage provides temporary block-level storage for your instance.   This storage is located on disks 
		that are physically attached to the host computer. Instance store is ideal for temporary storage of information 
		that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that 
		is replicated across a fleet of instances, such as a load-balanced pool of web servers." - From `AWS EC2 
		Instance Store <http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html>`_. 

		Use ephemeral storage as your main scratch workspace to temporarily store files needed for heavy I/O.  Ephemeral storage on the OSDC scales with the size of the instance.   We offer a number of Hi-Ephemeral flavors to 
		aid your research.   NB: In the case of the OSDC, the storage noted here only "persistents" for the life of the VM.   Once the VM is 
		terminated, the data stored here is lost.  Any snapshots made of your VM do NOT keep these data. 
		
		**Persistent**
		"Persistent storage means that the storage resource outlives other resources and is always available regardless 
		of the state of a running instance " - From `OpenStack documentation 
		<http://docs.openstack.org/openstack-ops/content/storage_decision.html>`_.   
		
		Any data you want to persist beyond the life of your VM or access from multiple VMs must be pushed to the S3-compatible object storage through the OSDC's Ceph Object Gateway.

What is OSDC Griffin?
-----------------------

OSDC Griffin is our newest compute resource named after the Chicago Architect Marion Mahony Griffin.  It is an OpenStack cluster utilizing ephemeral storage in VMs 
with access to a separate S3 storage system for persistent data storage.    OSDC Griffin has 610 cores, 2391 GiB of RAM, and 
369664 GiB of VM/ephemeral storage.  Allocations to all users and projects are given at the "tenant" level. 

Understanding Tenants 
-----------------------

Individual users share access to a pool of common compute resources within the overall quota of their group.  This group of users is called a "tenant."   
OSDC Griffin uses the tenant system to give groups of collaborating users maximum flexibility in managing their resource allocations.   

..  warning::
	
		Users could conceivably delete other users' data and VMs within a tenant.   BE VERY CAREFUL
		WHEN PERMANENTLY REMOVING DATA AND MANAGING VMS. 


Tenant Leaders
^^^^^^^^^^^^^^

When a project receives a resource allocation, one user expected to be the primary is assigned as the "tenant leader".   This individual 
is responsible for making sure other users in their tenant adhere to best practices and protocols they may wish to develop to 
govern their project's workflow. 

OSDC Griffin Flavors
----------------------

On OSDC Griffin, we offer 3 flavor families of VMs to facilitate the different types of 
use we've observed.    Standard flavors = m3.; Hi-Ephemeral for read/write disk I/O intensive applications with large files= he., and Hi-RAM for memory intensive applications= hr. 

Since Griffin is a community public resource, we ask that you only reserve the resources you need and terminate them when finished. 
 
  ===================    =============  ========  ===============  ============ ==================
  Family                 Flavor         VCPUs     Root P. (GiB)    RAM (GiB)    Eph Storage (GiB)      
  ===================    =============  ========  ===============  ============ ==================
  Standard               m3.small       1         10               3            32
  Standard               m3.medium      2         10               6            64
  Standard               m3.large       4         10               12           128
  Standard               m3.xlarge      8         10               24           256
  Standard               m3.xxlarge	16	  10	           48           512
  Standard               m3.xxxlarge    32        10	           96           1024
  I/O w/large files      he.medium      2         10               6            640
  I/O w/large files      he.large       4         10               12           896
  I/O w/large files      he.xlarge      8         10               24           1408
  I/O w/large files      he.xxlarge	16	  10	           48           2432
  Memory intensive       hr.medium      2         10               16           64
  Memory intensive       hr.large       4         10               32           128
  Memory intensive       hr.xlarge      8         10               64           256
  Memory intensive       hr.xxlarge	16	  10	           128          512
  ===================    =============  ========  ===============  ============ ==================


Accessing Griffin
-------------------
The table below has the addresses required to successfully ssh to the Griffin login node and any active VMs. 
For general instructions on how to manage VMs using the webconsole or managing ssh keys, please 
refer to the OSDC :doc:`Quickstart. <quickstart>`  


  ====================  =====================================================  ======================
  Cloud                 Login Node                             				  VM 
  ====================  =====================================================  ======================
  OSDC Griffin          ``<username>@griffin.opensciencedatacloud.org``        ``ubuntu@<VM.IP>`` 
  ====================  =====================================================  ======================


To work on the command line, please refer to the OSDC support 
on :doc:`Command Line Tools. <commandline>`

SSH Keypairs 
^^^^^^^^^^^^

It is necessary to have a keypair setup for both the login node and for instances.   This can be done using the webconsole 
by importing an ssh key as shown in :doc:`/ssh` or by command line.   You'll want to add the keypairs to your keychain and use the ``-A`` flag when sshing.   Please find full support instructions at :doc:`/ssh`.

EXAMPLE: Moving Files To VMs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here's an example script of how you could 'multihop' directly to a VM.   In order to take advantage 
of the multihop technique, below are some sample lines you could add to a 'config' file in your .ssh dir.   
On OSX this file is located or can be created in ``~/.ssh/config``.

.. code-block:: bash

    Host griffin
     HostName griffin.opensciencedatacloud.org
     IdentityFile ~/.ssh/<NAME OF YOUR PRIVATE KEY>
     User <OSDC USERNAME>
     
    THIS NEEDS WORK
    Host griffinvm
     HostName <VM IP>
     User ubuntu
     IdentityFile ~/.ssh/<NAME OF YOUR PRIVATE KEY>
     ProxyCommand ssh -q -A griffinssh -W %h:%p

You can then easily ssh into the headnode using ``ssh griffin`` and ``ssh griffinvm``. 

.. _griffinproxy:

Installing Software to Your VMs
----------------------------------------------

In order to keep OSDC Griffin a secure and compliant work environment, users must access outside resources through a proxy server.  

Accessing External Websites/Repositories
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to update or install packages via apt-get or to access external resources with tools like wget or curl you'll need
to go through a proxy server.   Add these lines to your VM's .bashrc file and source to update your current session:

.. code-block:: bash

    export no_proxy="griffin-objstore.opensciencedatacloud.org"
    function with_proxy() {
         PROXY='http://cloud-proxy:3128'
         http_proxy="${PROXY}" https_proxy="${PROXY}" $@
    }


Any time you need to access external sources, you must prepend the command with ``with_proxy`` and use ``sudo -E`` as part of your install/update commands.  For example,  instead of ``sudo apt-get update`` use ``with_proxy sudo -E apt-get update`` and instead of ``git clone https://github.com/LabAdvComp/osdc_support.git`` use ``with_proxy git clone https://github.com/LabAdvComp/osdc_support.git``

..  warning:: 
	
	If you do not take these steps and attempt to try commands that hit the internet w/o following the above, your session will hang and become unresponsive.
	
	If you are trying to access an external site and get a 403 error, the site is not currently on the 
	whitelist.   You'll need to request access for that site by sending an email to 
	support @ opensciencedatacloud dot org.


Understanding OSDC Griffin Storage Options
------------------------------------------

OSDC Griffin uses a combination of ephemeral storage in VMs and S3 object storage to
provide reliable and fast data storage devices.   In brief, best practices on Griffin involve:

NEED UPDATE

* Manage persistent data in S3 buckets.
* Grabbing data into VM ephemeral storage.
* Execute analysis, review result, delete any unnecessary data.
* Push results you wish to keep to S3.

END UPDATE

Using S3
^^^^^^^^


Workflow Guide
--------------

What follows is a step by step guide on how to work with ephemeral storage and S3 buckets to:

NEED UPDATE - below from PDC

* Create Cinder volumes and attach to a VM from the login node
* Mount Cinder volumes to a VM while in the VM
* Moving Cinder volumes
* Unmounting Cinder volumes
* Copy files and execute pipelines

END UPDATE 

EXAMPLES OF HOW TO DO ALL STEPS NOTED ABOVE
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Possible list of S3 commands?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

options?   S3cmd vs boto?

Accessing the Public Data Commons
---------------------------------
