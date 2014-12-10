Command Line Tools and Floating IPs
===================================

.. _commandline:

In addition to the OSDC console, there are a number of command line tools to manage virtual machines and cloud settings. 
The OpenStack tools have a variety of names, the primary one being 'nova'.  On Sullivan and the protected data clouds we do recommend using the 
OpenStack tools because they have a number of nice features. This page will describe some of the more common tasks, 
more details can be found on the `OpenStack <http://www.openstack.org/>`_ site.

List Images
--------------
OpenStack: ``nova image-list``

List Instance Types (Flavors)
------------------------------
OpenStack: ``nova flavor-list``

List Keypairs
--------------

OpenStack: ``nova keypair-list``

Start Instances
-------------------
OpenStack: ``nova boot <SERVER NAME> --image <IMAGE ID> --flavor <FLAVOR ID> --key_name <KEY NAME>``

The --user_data ~/.smbpasswd.sh provides the credentials to your VM to mount the GlusterFS share.
On Sullivan you can also manually mount /glusterfs after the fact by running '/cloudconf/mount-glusterfs $USERNAME'.  The password is located in your home directory at "smbpassword.txt"

List Instances
------------------
OpenStack: ``nova list``

Terminate Instances
----------------------
OpenStack: ``nova delete <INSTANCE ID>``

Create OSDC Images
--------------------
OpenStack
~~~~~~~~~
On the OpenStack clouds to create an image/snapshot:
  ``nova image-create <INSTANCE ID> <NEW UNIQUE NAME>``

It's a good idea to make the new image name informative.

By default the new images are private and can only be seen by you. To make images public:
  ``glance update <IMAGE ID> is_public=true image_type=snapshot``

Floating IPs
------------

Floating IPs are currently only available on the Sullivan cloud and in limited quantities.  
Please contact `support <support@opensciencedatacloud.org>`_ if you wish to obtain a floating IP.

Assign Floating IP to a VM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
*  Run ``nova list`` to get the VM's ID
*  Find free Floating IP with 'nova floating-ip-list'
*  Add IP to VM with ``nova add-floating-ip <server> <address>``


Remove Floating IP from a VM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
*  Run ``nova list`` to get the VM's ID
*  Remove IP from VM with ``nova remove-floating-ip <server> <address>``

Open Ports
~~~~~~~~~~~
The default security policy is already set to allow 22, 80 and 443 in this fashion::

    nova secgroup-add-rule default tcp 22 22   0.0.0.0/0
    nova secgroup-add-rule default tcp 80 80   0.0.0.0/0
    nova secgroup-add-rule default tcp 443 443   0.0.0.0/0
    nova secgroup-add-rule default icmp -1 -1   0.0.0.0/0




