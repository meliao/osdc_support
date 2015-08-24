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

Understanding OSDC Griffin Storage Options
------------------------------------------

OSDC Griffin uses a combination of Ephemeral storage in VMs and S3 object storage to
provide reliable and fast data storage devices.   Best practices on the Griffin involve:

NEED UPDATE
* Grabbing data (ie:  BAM file) from Swift into Cinder to insure fast I/O
* Execute pipelines and store intermediate files in Cinder
* Push results back to Swift
END UPDATE

OSDC Griffin Flavors
----------------------

  =============  ========  ===============  ============
  Flavor         VCPUs     VM Disk (GB)     RAM (GB)           
  =============  ========  ===============  ============
  m1.small       1         20               2          
  m1.medium      2         20               4         
  m1.large       4         20               8          
  m1.xlarge      8         20               16  
  m1.xxlarge	 16	   20	            48
  m1.xxxlarge    32        20	            96
  =============  ========  ===============  ============



General Griffin Use
-------------------
For general instructions on Griffin use, please refer to the OSDC 
:doc:`Quickstart. <quickstart>`  

To work on the command line, please refer to the OSDC support 
on :doc:`Command Line Tools. <commandline>`

.. _griffinproxy:

Installing Software
------------------------------

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


SSH Keypairs 
-----------------------
It is necessary to have a keypair setup for both the login node and for instances.   This can be done using the webconsole 
by importing an ssh key as shown in :doc:`/ssh` or by command line.   To do so from the command line, please refer to 
these `Openstack support docs <http://docs.openstack.org/user-guide/content/create_import_keys.html>`_.

It is likely you will just need to tell Nova about your keypairs which can be done using:

* ``nova keypair-add --pub_key ~/.ssh/id_rsa.pub KEY_NAME``

..  warning:: 
	
	If you plan to manage your ssh connections using Putty, please make sure that you are using v0.63 or beyond.   There are noted connection issues with older versions.

Moving Files To VMs
-----------------------

Here's an example script of for how you could 'multihop' direclty to the VM.   In order to take advantage 
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

Using S3
--------------


Workflow Guide
--------------

What follows is a step by step guide on how to work with Cinder and Swift to:

* Create Cinder volumes and attach to a VM from the login node
* Mount Cinder volumes to a VM while in the VM
* Moving Cinder volumes
* Unmounting Cinder volumes
* Copy files and execute pipelines

Copying OpenStack Environment Variables to VM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Currently, before running any swift command in your VM, you need to first copy ./novarc, 
which contains the OpenStack environment variables from head node to your VM, and source it.

In your head node:

* ``scp ~/.novarc ubuntu@<VM_IP>:/home/ubuntu``
* ``ssh ubuntu@<VM_IP>``
* ``source ~/.novarc``

If swift client is not installed, please get it via:

* ``sudo -E apt-get install python-swiftclient``

Swift Subcommands
^^^^^^^^^^^^^^^^^

A full list of Swift commands can be found in the `OpenStack user guide. <http://docs.openstack.org/user-guide/content/swift_commands.html>`_
Below are some sample commands you may find helpful for working with Swift.

* ``swift stat <CONTAINER_NAME> <OBJECT_NAME>`` 
	* Displays information for the account, container, or object
* ``swift list <CONTAINER_NAME> <OBJECT_NAME>``
	* Lists the objects for a container
	* If no <CONTAINER_NAME>, lists all containers for the account
*  ``swift delete <CONTAINER_NAME> <OBJECT_NAME>``
	* Deletes a container or objects within a container
* ``swift post <CONTAINER_NAME> <OBJECT_NAME>``
	* Updates meta information for the account, container, or object
	* If the container is not found, it will be created automatically
* ``swift upload <CONTAINER_NAME> <FILE_OR_DIRECTORY_NAME>``
	* Uploads files or directories to the given container
	* If the container is not found, it will be created automatically
	* If the file is larger than 5GB, you must use option ``--segment-size=SEGMENT_SIZE (-S SEGMENT_SIZE)``
		* NOTE:  Swift will upload files in segments no larger than <SEGMENT_SIZE> into a default container <CONTAINER_NAME>_segments, and then create a "manifest" file in the container <CONTAINER_NAME> that you can later use to download all the segments as if it were the original file.
* ``swift download <CONTAINER_NAME> <OBJECT_NAME>``
	* Download objects from containers

Some other useful options that can be used together with some (not all) of the subcommands

* help (-h): show help message
* verbose (-v): display/print more info
* lh: Report sizes in human readable format similar to ls -lh
* skip-identical: Skip uploading/downloading files that are identical on both sides

Examples of use:

* ``swift --help``
	* Shows help message for swift
* ``swift post --help``
	* Shows help message for swift post subcommand
* ``swift stat --verbose``
	* Displays more detailed information for the account
* ``swift list <CONTAINER_NAME>  --lh``
	* Lists all object in the container with sizes in readable format
* ``swift download <CONTAINER_NAME> --skip-identical``
	* Downloads all objects in the container to the current directory, and skip all files that is already in the directory
	


