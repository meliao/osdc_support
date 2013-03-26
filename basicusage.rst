Open Science Data Cloud Basic Usage
======================================
In your home directory on the login server (login1.opensciencedatacloud.org), there is a .euca subdirectory containing your EC2 (eucalyptus) credentials. This directory has mode 700 as it contains cryptographic keys for accessing EC2. Keep it protected.

To list images, run:

  ``euca-describe-images``

Look for entries with an ID starting with ‘emi-*’ or ‘ami-*’. OSDC-provided prototypical images are owned by admin and start with ‘prototypical-’.

Following types of VMs are supported:

+------------+----------+------------+-------------+
| VM Type    | Cores    | RAM (MB)   | Disk (GB)   |
|            |          |            |             |
+============+==========+============+=============+
| m1.small   | 1        | 3584       | 20          |
+------------+----------+------------+-------------+
| m1.large   | 2        | 716        | 20          |
+------------+----------+------------+-------------+
| c1.medium  | 4        | 14336      | 20          |
+------------+----------+------------+-------------+
| m1.xlarge  | 8        | 28672      | 20          |
+------------+----------+------------+-------------+
| c1.xlarge  | 8        | 28672      | 20          |
+------------+----------+------------+-------------+

To check the cloud availability/usage on the Adler cloud:

  ``euca-cloud-status``

To start an instance on Adler run:

  ``euca-run-instances -k USERNAME -n 1 -t m1.small emi-ID``

To start an instance on Sullivan run:

  ``euca-run-instances -k USERNAME -n 1 -t m1.small -f ~/.smbpasswd.sh emi-ID``

This will start an m1.small instance as defined by the command above. Note that it is best to start one instance at a time. That is, if one needs 20 instances, they should be started one-at-a-time followed by a delay and then the next instance, until all 20 are running. This effectively resolves Eucalyptus software scalability issues.

To monitor/list instances of one’s group (only!), run:

  ``euca-describe-instances``

This will return status of instances, with instance ID and an instance IP address.

To login to an instance on Adler run:

  ``ssh root@INSTANCE_IP``

To login to an instance on Sullivan run:

  ``ssh ubuntu@INSTANCE_IP``

To mount shared filesystem on Adler instances, run following command upon logging in:

  ``/cloudconf/mount-glusterfs USERNAME``

and enter your password.

For Sullivan instances the option -f ~/smbpasswd.sh ensures the shared filesystem is automatically mounted.

To check output log of an instance, run:

  ``euca-get-console-output INSTANCE_ID``

To stop an instance, log out of the instance and run this command:

  ``euca-terminate-instances INSTANCE_ID``



Open Science Data Cloud Image Features
-----------------------------------
Package installation:
OSDC Adler images are based on GNU/Debian 5.0 ‘Lenny’ amd64 Linux distribution. These images are pre configured to use our lab repository Debian cache service. 
OSDC Sullivan images are based on UEC cloud images with Ubuntu 12.04 ‘Precise’.

To find package names, run:

  ``apt-cache search KEYWORD1 KEYWORKD2 …``

To examine package info, run:

  ``aptitude show PACKAGENAME``

To install a package, run:

  ``aptitude install -PV PACKAGE-NAME1 PACKAGE-NAME2``
  


Creating Open Science Data Cloud images from an Adler instance
--------------------------------------------------------------
These instructions only apply to the Adler cloud. Please see below for Sullivan instructions.

  ``/cloudconf/mkimage-wrapper``

Make sure to use a unique image filename **every** time.

Copy your .euca/ directory from the login1.opensciencedatacloud.org server to the instance. On the login server run:

  ``rsync -avu .euca root@INSTANCE_IP:`` (*Be sure to include the colon*)
  
Source euca env (inside running VM):

  ``source ~/.euca/eucarc``
  
Bundle the image:

  ``euca-bundle-image -d /scratch/ -i /scratch/"new-unique-name-DATE-ENUM"``

Upload the bundle:

  ``euca-upload-bundle -b BUCKET_NAME -m /scratch/"new-unique-name-DATE-ENUM".manifest.xml``
  
Register the image:
  ``euca-register BUCKET_NAME/"new-unique-name-DATE-ENUM".manifest.xml``
  
Check if available:
  ``euca-describe-images``

Make sure to make a copy your MY-UPDATED-VM.img.manifest.xml file to the login1.opensciencedatacloud.org server, in order to easily remove an image at a later time. On the login server run:

  ``rsync -avu root@INSTANCE_IP:/scratch/"new-unique-name-DATE-ENUM".manifest.xml .``
  
To remove images (at a later time):

  ``euca-deregister emi-ID``
  
  ``euca-delete-bundle -b BUCKET_NAME -m ./"new-unique-name-DATE-ENUM".manifest.xml``



Creating Open Science Data Cloud images from a Sullivan instance
----------------------------------------------------------------
These instructions only apply to the Sullivan cloud. Please see above for Adler instructions.

Use the nova client to find the instance ID:

  ``nova list``
  
Then create the image:

  ``nova image-create INSTANCE_ID new-unique-name-DATE-ENUM``
  
By default these images are private and can only be seen by you. To make images public:

  ``glance update INSTANCE_ID is_public=true``
