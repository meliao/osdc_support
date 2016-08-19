Bionimbus PDC Resource Guide 
============================

.. _pdc:

Bionimbus PDC Best Practices
-----------------------------

The Bionimbus PDC is a HIPAA compliant cloud for analyzing and sharing protected data.   The Bionimbus PDC is an  OpenStack cluster utilizing ephemeral storage in VMs 
with access to a separate S3-compatible storage system for persistent data storage.  Allocations to all users and projects are given at the "tenant" level. 

Best practices on the PDC involve:

* Spin up a VM instance corresponding to your needs.
* Manage persistent data in the object store with S3.
* Pull data you need immediate access to into your VM's ephemeral storage, located in ``/mnt/``.
* Execute analysis, review result, delete any unnecessary local data.
* Push results and code you wish to keep to the S3-compatible object storage.
* Terminate your VM and, subsequently, the ephemeral storage. 

.. Topic:: 15 risky actions that could make Bionimbus PDC allocation grantees out of compliance with data use agreements.  
	
		* Share their credentials (ssh private key, password, s3 creds) with other people.
		* Taking protected data in a bucket and sharing with another user who may not have the same permissions.  
		* Save copies of their private key, password or s3 creds in insecure locations (don’t write them down on paper or save in an excel spreadsheet, don’t send them to any insecure device).
		* Send or share data with unsecured public or external devices (i.e., emailing data to your personal email, sending data to your unsecured home computer, sharing data in a bucket with users without approved access, etc).
		* Use an unencrypted laptop or mobile device.
		* Neglect to lock machine before leaving it.
		* Upload bootable images from untrusted sources (they could contain back doors or attempt to steal data).
		* Install software from untrusted sources.
		* Set up a public-facing system of any sort (forum, FTP, web site, etc) or use unlicensed or stolen commercial software.
		* Set up a unauthorized scanner of some sort.
		* Use outdated software that has security holes.
		* Visit or download from unknown sites/domains from any device within the PDC.
		* Fall for a phishing scheme.
		* Run a TOR server.
		* Use a Warez repository.

Understanding Tenants 
-----------------------

Individual users share access to a pool of common compute resources within the overall quota of their group.  This group of users is called a 
"tenant."   The Bionimbus PDC  uses the tenant system to give groups of collaborating users maximum flexibility in managing their resource allocations.   

..  warning::
	
		Users could conceivably delete other users' data and VMs within a tenant.   BE VERY CAREFUL
		WHEN PERMANENTLY REMOVING DATA AND MANAGING VMS. 


Tenant Leaders
^^^^^^^^^^^^^^

When a project receives a resource allocation, one user expected to be the primary is assigned as the "tenant leader".   This individual 
is responsible for making sure other users in their tenant adhere to best practices and protocols they may wish to develop to 
govern their project's workflow. 



Accessing the Bionimbus PDC
----------------------------
The table below has the addresses required to successfully ssh to the PDC login node and any active VMs. 
For general instructions on how to manage VMs using the webconsole or managing ssh keys, please 
refer to the OSDC :doc:`Quickstart. <quickstart>`  


  ====================  ========================================================  ======================
  Cloud                 Login Node                             				  VM 
  ====================  ========================================================  ======================
  Bionimbus PDC         ``<eRA.Commons>@bionimbus-pdc.opensciencedatacloud.org``  ``ubuntu@<VM.IP>`` 
  ====================  ========================================================  ======================

We maintain different storage hardware, with two different endpoints.   When your access is configured, you will be notified which endpoint to use.    

* Ceph Storage - ``https://bionimbus-objstore.opensciencedatacloud.org``  
* Cleversafe Storage - ``https://bionimbus-objstore-cs.opensciencedatacloud.org``

See :ref:`the s3 example <pdcs3example>` to learn how to access the object storage.

To work on the command line, please refer to the OSDC support 
on :doc:`Command Line Tools. <commandline>` 

.. _pdcproxy:

Connecting to External Sources
------------------------------

In order to keep the PDC a secure and compliant work environment, additional steps need to be taken anytime
you want to connect to an outside resource.  See the :ref:`whitelist <whitelist>` for a full list of currently 
available external sites. 

Working with the PDC Proxy Server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to update or install packages or to access external resources with tools like wget or curl you'll need
to work with a proxy server.   You'll need to take these steps every time you want to access external resources
or install or update packages. 

* Login to your VM
* Run ``export http_proxy=http://cloud-proxy:3128; export https_proxy=http://cloud-proxy:3128;``
* Swift endpoints are not whitelisted, so the best way to fix is to set ``export no_proxy="rados-bionimbus-pdc.opensciencedatacloud.org"``
* Access external sources - if installing, make sure and use ``sudo -E`` as part of your install/update commands
* Once completed, run:  ``unset http_proxy; unset https_proxy``

Updating .bashrc as a Workaround
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A helpful workaround is to add these lines to your VM's .bashrc file and source to update your current session:

.. code-block:: bash

    export no_proxy="bionimbus-objstore.opensciencedatacloud.org"
    function with_proxy() {
         PROXY='http://cloud-proxy:3128'
         http_proxy="${PROXY}" https_proxy="${PROXY}" $@
    }


Any time you need to access external sources, you must prepend the command with ``with_proxy`` and use ``sudo -E`` as part of your install/update commands.  For example,  instead of ``sudo apt-get update`` use ``with_proxy sudo -E apt-get update`` and instead of ``git clone https://github.com/LabAdvComp/osdc_support.git`` use ``with_proxy git clone https://github.com/LabAdvComp/osdc_support.git``

..  warning:: 
	
	If you do not take these steps, and attempt to try commands that hit the internet w/o running the above 
	commands to pull over settings from the proxy server, your session will hang and become unresponsive.
	
	If you are trying to access an external site and get a 403 error, the site is not currently on the 
	:ref:`whitelist <whitelist>`.   You'll need to request access for that site by sending an email to 
	support @ opensciencedatacloud dot org.

Using a Docker Image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To make the use of `Docker <https://www.docker.com/>`_ easier for users of the Bionimbus PDC and IGSB resource, a plain vanilla image is selectable from the Tukey console. The image has Docker installed from the official docker repo, but more importantly it's configured to use the proxy to get images (so you don't have to do anything), and it stores everything in /mnt, so users won't fill up their root, instead filling up the ephemeral storage available in the VM.   In the console, look for the public image called "docker_<date>".  

SSH Keypairs 
-----------------------
It is necessary to have a keypair setup for both the login node and for instances.   This can be done using the webconsole by importing an ssh key
or by command line.   To do so from the command line, please refer to 
these `Openstack support docs <http://docs.openstack.org/user-guide/content/create_import_keys.html>`_.

It is likely you will just need to tell Nova about your keypairs which can be done using:

* ``nova keypair-add --pub_key ~/.ssh/id_rsa.pub KEY_NAME``

..  warning:: 
	
	If you plan to manage your ssh connections using Putty, please make sure that you are using v0.63 or beyond.   There are noted connection issues with older versions.


Understanding Bionimbus PDC Storage Options and Workflow
---------------------------------------------------------

The Bionimbus PDC uses a combination of ephemeral storage in VMs and S3-compatible object storage to
provide reliable and fast data storage devices.   In brief, best practices on the Bionimbus PDC involve the following:

* Spin up a VM instance corresponding to your needs.
* Manage persistent data in the object store with S3.
* Pull data you need immediate access to into your VM's ephemeral storage, located in ``/mnt/``.
* Execute analysis, review result, delete any unnecessary local data.
* Push results and code you wish to keep to the S3-compatible object storage.
* Terminate your VM and, subsequently, the ephemeral storage. 

.. note:: **Storage types - Ephemeral vs. Persistent**
	
		**Ephemeral**
		"Ephemeral storage provides temporary block-level storage for your instance.   This storage is located on disks 
		that are physically attached to the host computer. Instance store is ideal for temporary storage of information 
		that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that 
		is replicated across a fleet of instances, such as a load-balanced pool of web servers." - From `AWS EC2 
		Instance Store <http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html>`_. 

		Use ephemeral storage as your main scratch workspace to temporarily store files needed for heavy I/O.  Ephemeral storage on the PDC  scales with the size of the instance.   We offer a number of Hi-Ephemeral flavors to 
		aid your research.   NB: In the case of the PDC, the storage noted here only "persistents" for the life of the VM.   Once the VM is 
		terminated, the data stored here is lost.  Any snapshots made of your VM do NOT keep these data. 
		
		**Persistent**
		"Persistent storage means that the storage resource outlives other resources and is always available regardless 
		of the state of a running instance " - From `OpenStack documentation 
		<http://docs.openstack.org/openstack-ops/content/storage_decision.html>`_.   
		
		Any data you want to persist beyond the life of your VM or access from multiple VMs must be pushed to the S3-compatible object storage through the PDC's Ceph or Cleversafe Object Gateways.


Setting Up /mnt on Ephemeral Storage VMs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
When starting a new VM with Ephemeral storage, users will need to change ownership of the storage to start.   In order to do so, login to the VM and run ``sudo chown ubuntu:ubuntu /mnt``.    Once complete you can begin to write or copy files to the ephemeral storage mounted to the VM.   This directory can with the command ``cd /mnt/``.  

EXAMPLE: Moving Files To VMs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here's an example of how you could use 'multihop' to directly get to a VM.   In order to take advantage 
of the multihop technique, below are some sample lines you could add to a 'config' file in your .ssh dir.   
On OSX this file is located or can be created in ``~/.ssh/config``.

.. code-block:: bash

    Host bionimbus
     HostName bionimbus-pdc.opensciencedatacloud.org
     IdentityFile ~/.ssh/<NAME OF YOUR PRIVATE KEY>
     User <eRA USERNAME>
     
    Host bionimbusvm
     HostName <VM IP>
     User ubuntu
     IdentityFile ~/.ssh/<NAME OF YOUR PRIVATE KEY>
     ProxyCommand ssh -q -A bionimbus -W %h:%p

You can then easily ssh into the headnode using ``ssh bionimbus`` or straight to your vm using ``ssh bionimbusvm``. You can also easily move files to the VMs ephemeral in a single command from your local machine using scp or rsync.  For example, from your local machine copy your favorite file to the ephemeral storage using ``scp myfavoritefile.txt bionimbusvm:/mnt/`` 

Using S3
^^^^^^^^

The PDC Object Gateways support a RESTful API that is basically compatible with Amazon's S3 API, with some limitations.  To push and pull data to the object storage, please refer to the `Ceph S3 API documentation <http://ceph.com/docs/master/radosgw/s3/>`_.  If a users wishes to write their own S3 object store interface, the support team recommends the Boto python library. Otherwise there is a precompiled tool released by Amazon called 'aws-cli'.  This is the recommended command line tool (CLI), we will not provide support for other S3 tools.  

To access the object storage via S3, you only need your S3 credentials (access key and secret key) and the name of the gateway.  S3 credentials are dropped into the home directory on the login node in a file named ``s3creds.txt``.  When users are removed from the tenant, this key is regenerated for security.  

There are 3 settings to access the S3 object store:

* ACCESS_KEY
* SECRET_KEY
* ENDPOINT_URL

The Keys can be found in the ``s3creds.txt`` file.   The ENDPOINT_URL is either: 

* Ceph Storage - ``https://bionimbus-objstore.opensciencedatacloud.org``  
* Cleversafe Storage - ``https://bionimbus-objstore-cs.opensciencedatacloud.org``

..  note:: 
	
	The S3 protocol requires that files larger than 5 GiB be 'chunked' in order to transfer into buckets.   Python boto supports these efforts using the `copy_part_from_key() method <http://docs.pythonboto.org/en/latest/ref/s3.html#boto.s3.multipart.MultiPartUpload.copy_part_from_key>`_. 

.. _pdcawscliexample:

EXAMPLE:   Using AWSCLI to interact with S3
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

aws-cli can be installed via the Python pip utility "pip installaws-cli", or the Ubuntu package utility "apt-get install awscli".   What follows is an example of how to setup a virtual environment in OSX with awscli installed (recommended to get past a common SSL error), configure environment with keys and tools, and then access data.  

For more information, reference the full `AWS CLI documentation <https://docs.aws.amazon.com/cli/latest/reference/s3/index.html>`_. 


.. code-block:: bash

		########################################################################
		### 1 ### create a python virtual environment (will take care of ssl error):

		brew install pyenv
		pyenv install 2.7.10
		sudo pip install virtualenvwrapper
		mkvirtualenv --python=~/.pyenv/versions/2.7.10/bin/python myPY2.7env
		pip install awscli

		# exit virtual environment
		deactivate
		# start virtual environment
		workon myPY2.7env
		########################################################################

		########################################################################
		### 2 ###

		# Get your credentials from PDC
		# log into the headnode
		# look for a file called "s3cred.txt"
		# get the contents

		less ~/s3cred.txt

		# will look something like this:

		[[tenant_namel]]
		access_key=USOMESTRINGOFCHARACTERSB
		secret_key=mANOTHERSTRINGOFCHARACTERSi

		# These are the keys you'll need to access the tenant
		# Note that our current policies do not accept sharing of keys.
		########################################################################

		########################################################################
		### 3 ### configure awscli

		# make sure you are in your virtual environment
		workon myPY2.7env

		aws configure --profile `my_project`

		# You will be queried to enter the access key from above
		# you can cut/paste the values and press enter

		AWS Access Key ID [****************]:

		# Do the same for your secret key

		AWS Secret Access Key [****************]:

		# Use 'us-east-1' as the default region name

		Default region name [us-east-1]: us-east-1
		#NOTE:  We will be ignoring this region and instead using one of our object store gateways.
		########################################################################
		### 4 ### work with data

		### Now you can use the following commands to access your data
		### beside that you specify the --endpoint-url, otherwise, awscli will try to contact amazon S3
		### below we are trying to hit the Ceph object store.   
		### If your profile storage is on Cleversafe, use 'https://bionimbus-objstore-cs.opensciencedatacloud.org' instead.
		### Also be sure to specify the profile

		# make a new bucket
		aws s3 mb s3://test-bucket --endpoint-url https://bionimbus-objstore.opensciencedatacloud.org --profile my_project
		make_bucket: s3://test-bucket/

		# list buckets
		aws s3 ls --endpoint-url https://bionimbus-objstore.opensciencedatacloud.org --profile my_project

		# list items in bucket
		aws s3 ls s3://test_bucket/ --endpoint-url https://bionimbus-objstore.opensciencedatacloud.org --profile my_project

		# copy a local file to the bucket
		aws s3 cp test_file s3://test-bucket/test_file --endpoint-url https://bionimbus-objstore.opensciencedatacloud.org --profile my_project

		# copy file from bucket to local
		aws s3 cp s3://test-bucket/testobject.txt testobject.txt --endpoint-url http://bionimbus-objstore.opensciencedatacloud.org --profile my_project

		# copy object from bucket to local
		aws s3 get-object s3://test-bucket/testobject.txt ./ --endpoint-url https://bionimbus-objstore.opensciencedatacloud.org --profile my_project
		########################################################################



.. _pdcs3example:

EXAMPLE:   Using Python's boto package to interact with S3
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Another way users can interact with the object storage via S3 is by using the Python boto package.   

Below is an example Python script for working with S3.  Generally, you will want to use the ephemeral mnt of your vm as your primary working directory.  In the example script below you will need to update the access_key and secret_key variables to the values in the s3creds.txt file, and the gateway variable to the correct gateway.    


.. code-block:: bash

	import boto
	import boto.s3.connection
	access_key = 'put your access key here!'	
	secret_key = 'put your secret key here!'
	bucket_name = 'put your bucket name here!'
	gateway = 'bionimbus-objstore.opensciencedatacloud.org'

	conn = boto.connect_s3(
        	aws_access_key_id = access_key,
        	aws_secret_access_key = secret_key,
        	host = gateway,
        	#is_secure=False,               # uncomment if you are not using ssl
        	calling_format = boto.s3.connection.OrdinaryCallingFormat(),
        	)

	### list buckets::
	for bucket in conn.get_all_buckets():
        	print "{name}\t{created}".format(
                	name = bucket.name,
                	created = bucket.creation_date,
        	)

	### create bucket::
	mybucket = conn.create_bucket(bucket_name)

	### creating an object directly::
	mykey = mybucket.new_key('testobject.txt')
	mykey.set_contents_from_string('working with s3 is fun')

	### load existing files to the object storage::
	files_to_put = ['myfavoritefile.txt','yourfavoritefile.txt']

	for k in files_to_put:
    		mykey = mybucket.new_key(k)
    		mykey.set_contents_from_filename(k)
	
	### list objects in bucket::
	for key in mybucket.list():
        	print "{name}\t{size}\t{modified}".format(
                	name = key.name,
                	size = key.size,
                	modified = key.last_modified,
                	)

	### downloading an object to local::
	mykey = bucket.get_key('testobject.txt')
	mykey.get_contents_to_filename('./testobject.txt')

	### deleting a bucket -- bucket must be empty::
	#conn.delete_bucket(bucket_name)

	### get existing bucket::
	mybucket = conn.get_bucket('my_bucket')


S3 Bucket Naming
^^^^^^^^^^^^^^^^
Bucket names must be unique across the entire system.   Please follow these constraints when creating a new bucket:

* Bucket names must be unique.
* Bucket names must begin and end with a lowercase letter.
* Bucket names should consist of only letters, numbers, dashes, and underscores

For more information consult the `Ceph documentation <http://docs.ceph.com/docs/master/radosgw/s3/bucketops/>`_ on buckets.  


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


