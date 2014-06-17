Bionimbus PDC Resource Guide 
============================

.. _pdc:

.. sidebar:: Storage types - Swift (Object) vs. Cinder (Block)
	
		**Swift:**
		Object storage provides access to whole objects, or blobs of data and generally 
		does so with an API specific to that system. Unlike file storage, object storage 
		generally does not allow the ability to write to one part of a file. Objects must 
		be updated as a whole unit. Object storage excels at storing content that can 
		grow without bound. One of the main advantages of object storage 
		systems is their ability to reliably store a large amount of data at relatively 
		low cost.
		
		**Cinder:**
		Block storage gives you access to the “bare metal”. There is no concept 
		of “files” at this level. There are just evenly sized blocks of data. Generally, 
		using block storage offers the best performance, but it is quite low-level. 
		Database servers often times can take advantage of block storage systems. 
		An example of a common block storage system is a SAN.
		
		*Condensed From* - `Rackspace Blog - Storage Systems Overview <http://www.rackspace.com/blog/storage-systems-overview/>`_

Bionimbus PDC Best Practices
-----------------------------

The Bionimbus PDC uses a combination of Cinder block storage and Swift object storage to
provide reliable and fast data storage devices.   Best practices on the PDC involve:

* Grabbing data (ie:  BAM file) from Swift into Cinder to insure fast I/O
* Execute pipelines and store intermediate files in Cinder
* Push results back to Swift

General PDC Use
----------------
For general instructions on PDC use, please refer to the OSDC 
:doc:`Quickstart. <quickstart>`  

To work on the command line, please refer to the OSDC support 
on :doc:`Command Line Tools. <commandline>`

CLI -SSH Keypairs BETA 
-----------------------
Until the Tukey console is available, keypairs to login to VMs will need to be managed from the command line.  To do so
please refer to these `Openstack support docs <http://docs.openstack.org/user-guide/content/create_import_keys.html>`_.

It is likely you will just need to tell Nova about your keypairs which can be done using:

* ``nova keypair-add --pub_key ~/.ssh/id_rsa.pub KEY_NAME``

Workflow Guide
--------------

What follows is a step by step guide on how to work with Cinder and Swift to:

* Create Cinder volumes and attach to a VM from the login node
* Mount Cinder volumes to a VM while in the VM
* Moving Cinder volumes
* Unmounting Cinder volumes
* Copy files and execute pipelines

CLI - Creating Cinder Volumes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First we'll create and attach Cinder volumes to VMs via the CLI.   This 
is done from the login node.  First ssh to the login node.

* ``ssh -A <username>@bionimbus-pdc.opensciencedatacloud.org``

Next, create a new VM. 

* ``nova boot --image <IMAGE_ID> --flavor <FLAVOR_NAME_OR_NUMBER VM_NAME> --key_name <KEYPAIR_NAME>``

A list of currently available VM Flavors is available below.

  =============  ========  ===============  ============
  Flavor         VCPUs     VM Disk (GB)     RAM (GB)           
  =============  ========  ===============  ============
  m1.tiny        1         0                .5          
  m1.small       1         20               2          
  m1.medium      2         20               4         
  m1.large       4         20               8          
  m1.xlarge      8         20               16               
  =============  ========  ===============  ============

We suggest creating three Cinder volumes, INPUT for input data, SCRATCH for intermediate 
scratch output, and OUTPUT for final results.  To create the following volumes with 
sample sizes (INPUT (1TB); SCRATCH (2TB); OUTPUT (1TB))

* ``nova volume-create --display-name INPUT 1024``
* ``nova volume-create --display-name SCRATCH 2048``
* ``nova volume-create --display-name OUTPUT 1024``

Next, list existing VMs and Cinder volumes and get the relevant UUID.  

* ``nova list``
* ``nova volume-list``

Finally, attached Cinder volumes to VMs.   This will need to be done for each Cinder volume.

* ``nova volume-attach <VM UUID> <CINDER VOL UUID>``  


CLI - Mounting Cinder Volumes to VM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Next we'll mount the volumes we created to ``mnt/cinder``.   Please note these can be mounted 
to other locations, or you can use symbolic links to your home dir for easy access.  After 
mounting, Cinder volumes can be used like regular folders, but with much faster I/O.

First login to your VM. 

* ``ssh ubuntu@<VM_IP>``

Next we'll want to make a directory, install xfs, construct xfs, and finally mount the Cinder 
volume.   The example below gives the commands to do so for the "INPUT" volume we created
earlier.  You'll want to repeat these commands for the "SCRATCH" AND "OUTPUT" volumes.

* ``sudo mkdir -p /mnt/cinder/INPUT``
* ``sudo apt-get -y install xfsprogs``
* ``sudo mkfs.xfs/dev/vdb``
* ``sudo mount /dev/vdb /mnt/cinder/INPUT/``

.. Topic:: Moving your Cinder Volume
	
		One of the advantages to working with Cinder volumes is that once you have the
		files you need in them, you can move them to other VMs.  To do so, follow the steps to 
		unmount listed below.   
		
		To remount them, follow the directions above, but make sure you don't reinstall xfs or run
		the mkfs command.   Doing so once your volume has been created would delete the contents.

CLI - Unmounting and Unattaching Cinder Volumes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once you have the information you'd like in a Cinder volume, you should detach it and unmount it.  
To unmount the "INPUT" volume example from above:

* ``sudo umount mnt/cinder/INPUT``

Then exit the VM, so you're back on the login node. 

* ``exit``

Then you'll want to detach the volume, so it can be reattached and remounted elsewhere.

* ``nova volume-detach <VM UUID> <CINDER VOL UUID>``

CLI - Copying Files, Executing Pipelines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We recommend you copy files from Swift to /mnt/cinder/input/, dump temp files into /mnt/cinder/scratch/,
write your output to /mnt/cinder/output/ and finally move your output back to your home dir on Swift.

Make sure your pipeline codes reflect these input, scratch, and output locations.   Please make sure and run 
your pipelines in Cinder volumes so that all temp files will be stored there.

Swift Commands
--------------
A full list of swift commands can be found in the `OpenStack user guide. <http://docs.openstack.org/user-guide/content/swift_commands.html>`_

Sample commands to mount TCGA data?
Need to install on VM?