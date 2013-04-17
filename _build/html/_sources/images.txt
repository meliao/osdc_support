Open Science Data Cloud Image Features
======================================

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

