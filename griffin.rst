Griffin Resource Guide 
============================

.. _griffin:

What is OSDC Griffin?
-----------------------

OSDC Griffin is named after the Chicago Architect Marion Mahony Griffin.  It is an OpenStack cluster utilizing ephemeral storage in VMs 
with access to a separate S3-compatible storage system for persistent data storage.    OSDC Griffin has 610 cores, 2391 GiB of RAM, and 
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

On OSDC Griffin, we offer 4 flavor families of VMs to facilitate the different types of 
use we've observed.    Standard flavors w/o Ephemeral storage = m1., Standard flavors w/ Ephemeral storage = m3., Hi-Ephemeral for read/write disk I/O intensive applications with large files = he., and Hi-RAM for memory intensive applications = hr. 

Since Griffin is a community public resource, we ask that you only reserve the resources you need and terminate them when finished. 
 
  ===================    =============  ========  ===============  ============ ==================
  Family                 Flavor         VCPUs     Root P. (GiB)    RAM (MiB)    Eph Storage (GiB)      
  ===================    =============  ========  ===============  ============ ==================
  No Ephemeral		 m1.small	1	  10		   2048		0
  No Ephemeral		 m1.medium	2	  10		   4096		0
  No Ephemeral		 m1.large	4	  10		   8192		0  
  No Ephemeral		 m1.xlarge	8	  10		   16384	0
  Standard               m3.small       1         10               3072         32
  Standard               m3.medium      2         10               6144         64
  Standard               m3.large       4         10               12288        128
  Standard               m3.xlarge      8         10               24576        256
  Standard               m3.xxlarge	16	  10	           49152        512
  Standard               m3.xxxlarge    32        10	           98304        1024
  I/O w/large files      he.medium      2         10               6144         640
  I/O w/large files      he.large       4         10               12288        896
  I/O w/large files      he.xlarge      8         10               24576        1408
  I/O w/large files      he.xxlarge	16	  10	           49152        2432
  Memory intensive       hr.medium      2         10               16384        64
  Memory intensive       hr.large       4         10               32768        128
  Memory intensive       hr.xlarge      8         10               65536        256
  Memory intensive       hr.xxlarge	16	  10	           131072       512
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

.. _griffinproxy:

Working with the Griffin Proxy
----------------------------------------------
In order to keep OSDC Griffin a secure and compliant work environment, users must access outside resources through a proxy server.

Accessing External Websites/Repositories
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to update or install packages via apt-get or to access external resources with tools like wget or curl you'll need
to go through a proxy server.   Add these lines to your VM's .bashrc file and source to update your current session.

1. Open your .bashrc file:
``vi ~/.bashrc``

2. Add these lines:

.. code-block:: bash

  export no_proxy="griffin-objstore.opensciencedatacloud.org"
  function with_proxy() {
    PROXY='http://cloud-proxy:3128'
    http_proxy="${PROXY}" https_proxy="${PROXY}" $@
  }

3. And then source the .bashrc file to make the changes active:
``source ~/.bashrc``



**ProTip:** Any time you need to access external sources, you must prepend the command with ``with_proxy`` and use ``sudo -E`` as part of your install/update commands.

For example,  instead of ``sudo apt-get update`` use ``with_proxy sudo -E apt-get update`` and instead of ``git clone https://github.com/LabAdvComp/osdc_support.git`` use ``with_proxy git clone https://github.com/LabAdvComp/osdc_support.git``

..  warning::

  If you do not take these steps and attempt to try commands that hit the internet w/o following the above, your session will hang and become unresponsive.

  If you are trying to access an external site and get a 403 error, the site is not currently on the
  whitelist.   You'll need to request access for that site by sending an email to
  support @ opensciencedatacloud dot org.

..  note::

  Once you have set your proxy, it's good practice when you are first spinning up a vanilla VM to run ``with_proxy sudo -E apt-get update`` to make sure you are starting with the latest packages.

Installing Software to Your VMs
----------------------------------------------

Using a Docker Image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To make the use of `Docker <https://www.docker.com/>`_ easier for Griffin users, a plain vanilla image is selectable from the Tukey console. The image has Docker installed from the official docker repo, but more importantly it's configured to use the proxy to get images (so you don't have to do anything), and it stores everything in /mnt, so users won't fill up their root, instead filling up the ephemeral storage available in the VM.   In the console, look for the public image called "docker_<date>".  

.. _install-jupyter:

EXAMPLE:  Installing Software and Running a Jupyter Notebook
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Section I:  Installing software
```````````````````````````````

1. Add Griffin proxies as variables in .bashrc as in the code block in on :ref:`using the proxy. <griffinproxy>`

	``printf "export no_proxy='griffin-objstore.opensciencedatacloud.org'\nfunction with_proxy() {\n\tPROXY='http://cloud-proxy:3128'\n\thttp_proxy=\"\${PROXY}\" https_proxy=\"\${PROXY}\" \$@\n}" >> ~/.bashrc``
2. Source .bashrc

	``. .bashrc``

3. Install Miniconda

	``with_proxy wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh1``

	``bash Miniconda2-latest-Linux-x86_64.sh -b``

4. Install requisite Python modules

	``with_proxy conda update conda``

	``with_proxy conda install jupyter``

.. Note::
   
   **ProTip:**  Running ``with_proxy conda clean -tipsy`` after installation of conda packages removes the (now unnecessary) intermediates, freeing up valuable disk space.  

Section II: How to run Jupyter Notebook
```````````````````````````````````````

1. *From your Local Machine (e.g. your laptop)*:add following lines to ~/.ssh/config (replacing \<OSDC username\> with your OSDC username)

        ``Host 172.17.*``
	``User ubuntu``
	``ProxyCommand ssh <OSDC username>@griffin.opensciencedatacloud.org nc %h %p 2> /dev/null``

2. *From Griffin VM*

        ``jupyter notebook --no-browser``

3. *From your Local Machine*

        ``ssh -L <local port>:localhost:<griffin port> -N <Griffin VM IP Address>``

	replacing \<griffin port\> with \<port\> given in `jupyter notebook --no-browser` output as
	
	> The Jupyter Notebook is running at: http://localhost:<port>/	

4. *From your Local Machine*

    open browser, enter `http://localhost:<local port>`


Understanding OSDC Griffin Storage Options and Workflow
---------------------------------------------------------

OSDC Griffin uses a combination of ephemeral storage in VMs and S3-compatible object storage to
provide reliable and fast data storage devices.   In brief, best practices on Griffin involve the following:

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

		Use ephemeral storage as your main scratch workspace to temporarily store files needed for heavy I/O.  Ephemeral storage on the OSDC scales with the size of the instance.   We offer a number of Hi-Ephemeral flavors to 
		aid your research.   NB: In the case of the OSDC, the storage noted here only "persistents" for the life of the VM.   Once the VM is 
		terminated, the data stored here is lost.  Any snapshots made of your VM do NOT keep these data. 
		
		**Persistent**
		"Persistent storage means that the storage resource outlives other resources and is always available regardless 
		of the state of a running instance " - From `OpenStack documentation 
		<http://docs.openstack.org/openstack-ops/content/storage_decision.html>`_.   
		
		Any data you want to persist beyond the life of your VM or access from multiple VMs must be pushed to the S3-compatible object storage through the OSDC's Ceph Object Gateway.


Setting Up /mnt on Ephemeral Storage VMs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
When starting a new VM with Ephemeral storage, users will need to change ownership of the storage to start.   In order to do so, login to the VM and run ``sudo chown ubuntu:ubuntu /mnt``.    Once complete you can begin to write or copy files to the ephemeral storage mounted to the VM.   This directory can with the command ``cd /mnt/``.  

EXAMPLE: Moving Files To VMs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here's an example of how you could use 'multihop' to directly get to a VM.   In order to take advantage 
of the multihop technique, below are some sample lines you could add to a 'config' file in your .ssh dir.   
On OSX this file is located or can be created in ``~/.ssh/config``.

.. code-block:: bash

    Host griffin
     HostName griffin.opensciencedatacloud.org
     IdentityFile ~/.ssh/<NAME OF YOUR PRIVATE KEY>
     User <OSDC USERNAME>
     
    Host griffinvm
     HostName <VM IP>
     User ubuntu
     IdentityFile ~/.ssh/<NAME OF YOUR PRIVATE KEY>
     ProxyCommand ssh -q -A griffin -W %h:%p

You can then easily ssh into the headnode using ``ssh griffin`` or straight to your vm using ``ssh griffinvm``. You can also easily move files to the VMs ephemeral in a single command from your local machine using scp or rsync.  For example, from your local machine copy your favorite file to the ephemeral storage using ``scp myfavoritefile.txt griffinvm:/mnt/`` 

Using S3
^^^^^^^^

The OSDC Ceph Object Gateway supports a RESTful API that is basically compatible with Amazon's S3 API, with some limitations.  To push and pull data to the object storage, please refer to the `Ceph S3 API documentation <http://ceph.com/docs/master/radosgw/s3/>`_.  If a users wishes to write their own S3 object store interface, the support team recommends the Boto python library. Otherwise there is a precompiled tool released by Amazon called 'aws-cli'.  This is the recommended command line tool (CLI), we will not provide support for other S3 tools.  

To access the object storage via Ceph's S3, you only need your S3 credentials (access key and secret key) and the name of the gateway.  S3 credentials are dropped into the home directory on the login node in a file named ``s3creds.txt``.  When users are removed from the tenant, this key is regenerated for security.  

There are 3 settings to access the S3 object store:

* ACCESS_KEY 
* SECRET_KEY
* ENDPOINT_URL

The Keys can be found in the ``s3creds.txt`` file. The ENPOINT_URL for the object store is "griffin-objstore.opensciencedatacloud.org".

..  note:: 
	
	The S3 protocol requires that files larger than 5 GiB be 'chunked' in order to transfer into buckets.   Python boto supports these efforts using the `copy_part_from_key() method <http://docs.pythonboto.org/en/latest/ref/s3.html#boto.s3.multipart.MultiPartUpload.copy_part_from_key>`_. 


.. _griffinawscliexample:

EXAMPLE:   Using AWSCLI to interact with S3 from a Griffin Ubuntu Instance
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

..  warning::

  The Ubuntu package awscli installed via apt-get is out of date. Do not install this! Instead using the python pip install command detailed below.

For more information, reference the full `AWS CLI documentation <https://docs.aws.amazon.com/cli/latest/reference/s3/index.html>`_. 


.. code-block:: bash

		########################################################################
		### 1 ### Update apt-get, install Python PIP if you haven't already and then pip install awscli

		sudo apt-get update
		sudo apt-get upgrade
		sudo apt-get install python-pip
		with_proxy sudo -E pip install awscli

		########################################################################

		########################################################################
		### 2 ###

		# Get your credentials from Griffin
		# log into the headnode (griffin.opensciencedatacloud.org)
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

		aws configure --profile `my_project`

		# You will be queried to enter the access key from above
		# you can cut/paste the values and press enter

		AWS Access Key ID [****************]:

		# Do the same for your secret key

		AWS Secret Access Key [****************]:

		# Use 'us-east-1' as the default region name, awscli may not work if you do not specify a region name

		Default region name [us-east-1]: us-east-1
		#NOTE:  We will be ignoring this region and instead using one of our object store gateways.
		########################################################################
		### 4 ### work with data

		### Now you can use the following commands to access your data
		### beside that you specify the --endpoint-url, otherwise, awscli will try to contact amazon S3
		### Also be sure to specify the profile

		# make a new bucket
		aws s3 mb s3://test-bucket --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project
		make_bucket: s3://test-bucket/

		# list buckets
		aws s3 ls --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# list items in bucket
		aws s3 ls s3://test_bucket/ --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# copy a local file to the bucket
		aws s3 cp test_file s3://test-bucket/test_file --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# copy file from bucket to local
		aws s3 cp s3://test-bucket/testobject.txt testobject.txt --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# copy object from bucket to local
		aws s3 get-object s3://test-bucket/testobject.txt ./ --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project
		########################################################################



EXAMPLE:   Using Python's boto package to interact with S3
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Another way users can interact with the object storage via S3 is by using the Python boto package.   

Below is an example Python script for working with S3.  Generally, you will want to use the ephemeral mnt of your vm as your primary working directory.  In the example script below you will need to update the access_key and secret_key variables to the values in the s3cred.txt file.    


.. code-block:: bash

	import boto
	import boto.s3.connection
	access_key = 'put your access key here!'	
	secret_key = 'put your secret key here!'
	bucket_name = 'put your bucket name here!'
	gateway = 'griffin-objstore.opensciencedatacloud.org'

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
    		key = mybucket.new_key(k)
    		key.set_contents_from_filename(k)
	
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


	### Set public read to all objects in a bucket::
	for key in mybucket.list():
	    key.set_acl('public-read')


EXAMPLE:   Using AWSCLI to interact with S3 from OS X
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

What follows is an example of how to setup a virtual environment in OSX with awscli installed (recommended to get past a common SSL error), configure environment with keys and tools, and then access data.

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

		# Get your credentials from Griffin
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
		### Also be sure to specify the profile

		# make a new bucket
		aws s3 mb s3://test-bucket --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project
		make_bucket: s3://test-bucket/

		# list buckets
		aws s3 ls --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# list items in bucket
		aws s3 ls s3://test_bucket/ --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# copy a local file to the bucket
		aws s3 cp test_file s3://test-bucket/test_file --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# copy file from bucket to local
		aws s3 cp s3://test-bucket/testobject.txt testobject.txt --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project

		# copy object from bucket to local
		aws s3 get-object s3://test-bucket/testobject.txt ./ --endpoint-url https://griffin-objstore.opensciencedatacloud.org --profile my_project
		########################################################################


S3 Bucket Naming
^^^^^^^^^^^^^^^^
Bucket names must be unique across the entire system.   Please follow these constraints when creating a new bucket:

* Bucket names must be unique.
* Bucket names must begin and end with a lowercase letter.
* Bucket names should consist of only letters, numbers, dashes, and underscores

For more information consult the `Ceph documentation <http://docs.ceph.com/docs/master/radosgw/s3/bucketops/>`_ on buckets.  


Making a Bucket Public
^^^^^^^^^^^^^^^^^^^^^^^

Some Griffin users may wish to publicly expose their buckets to share data with collaborators or with the OSDC community as part of the public data commons.    To do so, simply set the bucket acls to "public-read".   An example using boto is below. 

.. code-block:: bash
		
	### get existing bucket::
	mybucket = conn.get_bucket('my_bucket')

	### Set public read to all objects in a bucket::
	for key in mybucket.list():
            key.set_acl('public-read')


Once you have set the object acls to 'public-read' the data is accessible in a browser at ``https://griffin-objstore.opensciencedatacloud.org/<my_bucket>/<object_name>``.   You will need to specify the entire path for each object.   
                

Accessing the Public Data Commons
---------------------------------

The current version of the OSDC Public Data Commons is not automounted to Griffin virtual machines.  To access any data hosted there, refer to the external download instructions on the `OSDC Public Data Commons webpages <https://www.opensciencedatacloud.org/publicdata/>`_.
