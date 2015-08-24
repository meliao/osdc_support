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

		Ephemeral storage sizes on the OSDC scale with the size of the instance.   We offer a number of HI-Ephemeral flavors to 
		aid your research.   In the case of the OSDC, the storage noted here is "persistent" for the life of the VM.   Once the VM is 
		terminated, the data stored here will go away.    QUESTION TO CONFIRM - WILL IT STAY IF SNAPSHOTTED, TERMINATED, AND SPUN BACK UP? 
		
		**Persistent**
		"Persistent storage means that the storage resource outlives other resources and is always available regardless 
		of the state of a running instance " - From `OpenStack documentation 
		<http://docs.openstack.org/openstack-ops/content/storage_decision.html>`_.   
		
		OSDC Griffin uses the Ceph Object Gateway to provide users to access to S3-compatible object storage.

What is OSDC Griffin?
-----------------------

OSDC Griffin is our newest compute resource named after the Chicago Architect Marion Griffin.  It utilizes ephemeral storage in VMs 
and connects to a separate S3 storage system for persistent user storage.    OSDC Griffin has 608 cores, 2391 GiB of RAM, and 
369664 GiB of storage used in VMs for root partitioning and ephemeral stores.  Users and projects are managed at the tenant level. 

Understanding Tenants 
-----------------------

In Openstack tenants allow the OSDC systems team to manage groups of users and projects be providing common resources, tools, and quotas.   
OSDC Griffin uses the tenant system to give the users maximum flexibility in managing their resource allocations.   

..  warning::
	
		In shared tenants, users could conceivably delete other users' data and VMs.   BE VERY CAREFUL
		WHEN PERMENTLY REMOVING DATA AND MANAGING TENANT VMS. 


Tenant Leaders
^^^^^^^^^^^^^^

When a project receives a resource allocation one user expected to be the primary is assigned as the "tenant leader".   This individual 
needs to be responsible for making sure other users in their tenant adhere to best practices and protocols they may wish to develop to 
govern their project's workflow. 

OSDC Griffin Flavors
----------------------

On OSDC Griffin, we offer 3 flavor families of VMs to facilitate the different types of 
use we've observed.    Standard flavors = .m3; Hi-Ephemeral = .he, and Hi-RAM = .hr. 

Since Griffin is a community public resource, we ask that you only reserve the resources you need. 
 
  =============  ========  ===============  ============ ==================
  Flavor         VCPUs     Root P. (GiB)    RAM (GiB)    Eph Storage (GiB)      
  =============  ========  ===============  ============ ==================
  m3.small       1         10               3            32
  m3.medium      2         10               6            64
  m3.large       4         10               12           128
  m3.xlarge      8         10               24           256
  m3.xxlarge	 16	   10	            48           512
  m3.xxxlarge    32        10	            96           1024
  he.medium      2         10               6            640
  he.large       4         10               12           896
  he.xlarge      8         10               24           1408
  he.xxlarge	 16	   10	            48           2432
  hr.medium      2         10               16           64
  hr.large       4         10               32           128
  hr.xlarge      8         10               64           256
  hr.xxlarge	 16	   10	            128          512
  =============  ========  ===============  ============ ==================


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
by importing an ssh key as shown in :doc:`/ssh` or by command line.   To do so from the command line, please refer to 
these `Openstack support docs <http://docs.openstack.org/user-guide/content/create_import_keys.html>`_.

It is likely you will just need to tell Nova about your keypairs which can be done using:

* ``nova keypair-add --pub_key ~/.ssh/id_rsa.pub KEY_NAME``

..  warning:: 
	
	If you plan to manage your ssh connections using Putty, please make sure that you are using v0.63 or beyond.   There are noted connection issues with older versions.

EXAMPLE: Moving Files To VMs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here's an example script of for how you could 'multihop' directly to the VM.   In order to take advantage 
of the multihop technique, below are some sample lines you could add to a 'config' file in your .ssh dir.   
On OSX this file is located or can be created in ``/Users/username/.ssh/config``.

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

Installing Software and Using the Proxy Server
----------------------------------------------

In order to keep OSDC Griffin a secure and compliant work environment, additional steps need to be taken anytime
you want to connect to an outside resource.  

Working with the Griffin Proxy Server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to update or install packages or to access external resources with tools like wget or curl you'll need
to work with a proxy server.   You'll need to take these steps every time you want to access external resources
or install or update packages. 

* Login to your VM
* Run ``export http_proxy=http://cloud-proxy:3128; export https_proxy=http://cloud-proxy:3128;``
* Swift endpoints are not whitelisted, so the best way to fix is to set ``export no_proxy="griffin-objstore.opensciencedatacloud.org"``
* Access external sources - if installing, make sure and use ``sudo -E`` as part of your install/update commands
* Once completed, run:  ``unset http_proxy; unset https_proxy``

..  warning:: 
	
	If you do not take these steps, and attempt to try commands that hit the internet w/o running the above 
	commands to pull over settings from the proxy server, your session will hang and become unresponsive.
	
	If you are trying to access an external site and get a 403 error, the site is not currently on the 
	whitelist.   You'll need to request access for that site by sending an email to 
	support @ opensciencedatacloud dot org.


Understanding OSDC Griffin Storage Options
------------------------------------------

OSDC Griffin uses a combination of Ephemeral storage in VMs and S3 object storage to
provide reliable and fast data storage devices.   In brief, best practices on Griffin involve:

NEED UPDATE

* Manage persistant data in S3 buckets.
* Grabbing data into VM ephmeral storage.
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
