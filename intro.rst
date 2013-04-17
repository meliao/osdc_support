Introduction to the Open Science Data Cloud
===========================================

The `Open Science Data Cloud <https://opensciencedatacloud.org>`_ is a distributed, cloud-based infrastructure for managing, analyzing, archiving and sharing scientific datasets.



"Big-Picture" Diagram of OSDC Infrastructure
--------------------------------------------

.. figure:: OSDCinfrastructure.png
   :height: 392
   :width: 531
   :align: center

   Figure illustrating OSDC infrastructure

   Figure legend: 




   +-----------------------+-------------------------------------------------------------------------------+
   | Element               | Description								   |
   +=======================+===============================================================================+
   |     		   |										   |
   | .. image:: user.png   | You (internet user)							   |
   |  	:width: 30         |		   								   |
   |	:height: 29        |                         							   |
   |	                   |										   |	
   |			   |										   |
   |		   |                        							   |
   +-----------------------+-------------------------------------------------------------------------------+
   | **Adler**             | `Eucalyptus <http://www.eucalyptus.com/eucalyptus-cloud/iaas>`_ based cloud   |
   +-----------------------+-------------------------------------------------------------------------------+
   | **Sullivan**          | `OpenStack <http://www.openstack.org/>`_ based cloud                          |
   +-----------------------+-------------------------------------------------------------------------------+
   | **GlusterFS**         | Shared file system local to the cloud                                         |
   +-----------------------+-------------------------------------------------------------------------------+
   | **Root**              | Public data storage location shared by the clouds                             |
   +-----------------------+-------------------------------------------------------------------------------+

<<<<<<< HEAD
=======

.. image:: _static/OSDCinfrastructure.png
   :height: 784
   :width: 1062
   :scale: 45
   :alt: "Big-Picture" Diagram of OSDC Infrastructure

``Internet (users)``
>>>>>>> 4e42cf3e9d3f08da39a6507b72ace7b1fd1a0bcc

``ssh -A login1.opensciencedatacloud.org (Adler)`` or ``ssh -A login2.opensciencedatacloud.org (Sullivan)``

``Open Science Data Cloud (VMs) -->-- NAT -->-- Internet``

