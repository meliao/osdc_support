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

.. _pdcproxy:

Connecting to External Sources
------------------------------

In order to keep the PDC a secure and compliant work environment, additional steps need to be taken anytime
you want to connect to an outside resource.  See the :ref:`whitelist <whitelist>` for a full list of currently 
whitelisted external sites. 

Working with the PDC Proxy Server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to update or install packages or to access external resources with tools like wget or curl you'll need
to work with our proxy server.   You'll need to take these steps every time you need to access external resources
or install or update packages. 

* Login to your VM
* Run ``export http_proxy=http://cloud-proxy:3128; export https_proxy=http://cloud-proxy:3128;``
* Access External Sources
* Once completed, run:  ``unset http_proxy; unset https_proxy``

.. NOTE:: If you do not take these steps, and attempt to try commands that hit the internet w/o running the above 
	commands to pull over settings from the proxy server, your session will hang and become unresponsive.


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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First we'll create and attach Cinder volumes to VMs via the CLI.   This 
is done from the login node.  First ssh to the login node.

* ``ssh -A <username>@bionimbus-pdc.opensciencedatacloud.org``

Next, create a new VM. 

* ``nova boot --image <IMAGE_ID> --flavor <FLAVOR_NAME_OR_NUMBER> --key_name <KEYPAIR_NAME> <VM_NAME>``

A list of currently available VM Flavors is available below.

  =============  ========  ===============  ============
  Flavor         VCPUs     VM Disk (GB)     RAM (GB)           
  =============  ========  ===============  ============
  m1.small       1         20               2          
  m1.medium      2         20               4         
  m1.large       4         20               8          
  m1.xlarge      8         20               16  
  m1.xxlarge	 16	       20	            48
  m1.xxxlarge    32        20	            96
  =============  ========  ===============  ============

We suggest creating a 1TB Cinder volume called SCRATCH for intermediate 
scratch output.  

* ``nova volume-create --display-name SCRATCH 1024``

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
volume.   The example below gives the commands to do so for the "SCRATCH" volume we created
earlier.  

* ``sudo mkdir -p /mnt/cinder/SCRATCH``
* ``sudo apt-get -y install xfsprogs``
* ``sudo mkfs.xfs /dev/vdb``
* ``sudo mount /dev/vdb /mnt/cinder/SCRATCH/``

.. Topic:: Moving your Cinder Volume
	
		One of the advantages to working with Cinder volumes is that once you have the
		files you need in them, you can move them to other VMs.  To do so, follow the steps to 
		unmount listed below.   
		
		To remount them, follow the directions above, but make sure you don't reinstall xfs or run
		the mkfs command.   Doing so once your volume has been created would delete the contents.

CLI - Unmounting and Unattaching Cinder Volumes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once you have the information you'd like in a Cinder volume, you should detach it and unmount it.  
To unmount the "SCRATCH" volume example from above:

* ``sudo umount mnt/cinder/SCRATCH``

Then exit the VM, so you're back on the login node. 

* ``exit``

Then you'll want to detach the volume, so it can be reattached and remounted elsewhere.

* ``nova volume-detach <VM UUID> <CINDER VOL UUID>``

CLI - Copying Files, Executing Pipelines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We recommend you copy files, dump temp files, and write your output from Swift to /mnt/cinder/scratch/, 
and finally move your output back to your home dir on Swift. Make sure your pipeline codes reflect 
this scratch location.   Please make sure and run your pipelines in Cinder volumes so that all 
temp files will be stored there.

Using Swift
--------------

Copying OpenStack Environment Variables to VM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Currently, before running any swift command in your VM, you need to first copy ./novarc, 
which contains the OpenStack environment variables from head node to your VM, and source it.

In your head node:

* ``scp ~/.novarc ubuntu@<VM_IP>:/home/ubuntu``
* ``ssh ubuntu@<VM_IP>``
* ``source ~/.novarc``

If swift client is not installed, please get it via:

* ``sudo apt-get install python-swiftclient``

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
	
.. _whitelist:
	
Whitelisted Resources
---------------------

Below is a growing list of resources currently whitelisted on the PDC.   If a site with tools you need is 
not listed below, please open up a ticket with support @ opensciencedatacloud dot org.

Debian/Ubuntu Mirrors
^^^^^^^^^^^^^^^^^^^^^^

* archive.ubuntu.com
* security.ubuntu.com
* mirror.anl.gov
* security.debian.org
* http.us.debian.org
* keyserver.ubuntu.com
* mirror.csclub.uwaterloo.ca
* us.archive.ubuntu.com
* ppa.launchpad.net

Cghub
^^^^^^^^^^^^^^^^^^^^^^

* cghub.ucsc.edu

Git
^^^^^^^^^^^^^^^^^^^^^^
* source.bionimbus.org
* git.bionimbus.org
* .github.com

OpenID
^^^^^^^^^^^^^^^^^^^^^^
* www.google.com

ClamAV
^^^^^^^^^^^^^^^^^^^^^^

* db.local.clamav.net

Pypi
^^^^^^^^^^^^^^^^^^^^^^

* .pypi.python.org

Bioconductor
^^^^^^^^^^^^^^^^^^^^^^

* .bioconductor.org
* bioconductor.org

R mirrors
^^^^^^^^^^^^^^^^^^^^^^

* cran.r-project.org
* cran.cnr.Berkeley.edu
* cran.stat.ucla.edu
* streaming.stat.iastate.edu
* ftp.ussg.iu.edu
* rweb.quant.ku.edu
* watson.nci.nih.gov
* cran.mtu.edu
* cran.wustl.edu
* cran.case.edu
* ftp.osuosl.org
* lib.stat.cmu.edu
* mirrors.nics.utk.edu
* cran.fhcrc.org
* cran.cs.wwu.edu

Perl/CPAN mirrors
^^^^^^^^^^^^^^^^^^^^^^

* cpan.mirrors.tds.net
* .cpan.org
* .bitbucket.org
* .perl.org
* .metacpan.org

SourceForge
^^^^^^^^^^^^^^^^^^^^^^

* .sourceforge.net


