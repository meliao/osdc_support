Command Line Tools
=====================

In addition to the OSDC console, there are a number of command line tools to manage virtual machines and cloud settings. The Eucalyptus tools (euca2ools) all start with 'euca-' while the OpenStack tools have a variety of names, the primary one being 'nova'. The euca2ools can be used on the OpenStack based clouds, but the OpenStack tools can not be used on Eucalyptus based clouds. On Sullivan and the protected data clouds we do recommend using the OpenStack tools because they have a number of nice features. This page will describe some of the more common tasks, more details can be found on the `Eucalyptus <http://www.eucalyptus.com/>`_ and `OpenStack <http://www.openstack.org/>`_ sites.

List Images
--------------
OpenStack: ``nova image-list``

Eucalyptus: ``euca-describe-images``


List Instance Types (Flavors)
------------------------------
OpenStack: ``nova flavor-list``

Eucalyptus: ``euca-cloud-status``

List Keypairs
--------------

OpenStack: ``nova keypair-list``

Eucalyptus: ``euca-describe-keypairs``

Start Instances
-------------------
OpenStack: ``nova boot <SERVER NAME> --image <IMAGE ID> --flavor <FLAVOR ID> --key_name <KEY NAME>``

The --user_data ~/.smbpasswd.sh provides the credentials to your VM to mount the GlusterFS share.
On Sullivan you can also manually mount /glusterfs after the fact by running '/cloudconf/mount-glusterfs $USERNAME'.  The password is located in your home directory at "smbpassword.txt"

Eucalyptus: ``euca-run-instances -k <USER NAME> -n 1 -t <INSTANCE TYPE> <EMI ID>``

List Instances
------------------
OpenStack: ``nova list``

Eucalyptus: ``euca-describe-instances``

Terminate Instances
----------------------
OpenStack: ``nova delete <INSTANCE ID>``

Eucalyptus: ``euca-terminate-instances <INSTANCE ID>``

Create OSDC Images
--------------------
OpenStack
~~~~~~~~~
On the OpenStack clouds to create an image/snapshot:
  ``nova image-create <INSTANCE ID> <NEW UNIQUE NAME>``

It's a good idea to make the new image name informative.

By default the new images are private and can only be seen by you. To make images public:
  ``glance update <IMAGE ID> is_public=true image_type=snapshot``

Adler (Eucalyptus)
~~~~~~~~~~~~~~~~~~
Creating an image on Adler requires a special process.

On the running VM run:
  ``/cloudconf/mkimage-wrapper``

**Note**: mkimage-wrapper will ask for a disk image name, make sure to use a unique image filename **EVERY** time

Copy your .euca/ directory from the login1.opensciencedatacloud.org server to the instance. On the login server run:

  ``rsync -avu .euca root@INSTANCE_IP:`` (**Be sure to include the colon**)

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



Managing Floating IPs 
----------------------
Floating IPs are currently only available on the Sullivan cloud and in limited quantities.  
Please contact `support <support@opensciencedatacloud.org>`_ if you wish to obtain a floating IP.


Assign Floating IP to a VM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
*  Run ``nova list`` to get the VM's ID
*  Find free Floating IP with 'nova floating-ip-list'
*  Add IP to VM with ``nova add-floating-ip ID FLOATING_IP``


Remove Floating IP from a VM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
*  Run ``nova list`` to get the VM's ID
*  Remove IP from VM with ``nova remove-floating-ip UUID FLOATING_IP``

Open Ports
~~~~~~~~~~~
The default security policy is already set to allow 22, 80 and 443 in this fashion::

    nova secgroup-add-rule default tcp 22 22   0.0.0.0/0
    nova secgroup-add-rule default tcp 80 80   0.0.0.0/0
    nova secgroup-add-rule default tcp 443 443   0.0.0.0/0
    nova secgroup-add-rule default icmp -1 -1   0.0.0.0/0


