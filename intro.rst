Introduction to the Open Science Data Cloud
===========================================

The `Open Science Data Cloud <www.opensciencedatacloud.org>`_ is a distributed, cloud-based infrastructure for managing, analyzing, archiving and sharing scientific datasets.



"Big-Picture" Diagram of OSDC Infrastructure
--------------------------------------------

The Open Science Data Cloud is currently composed of **Adler**, a cloud based on Eucalyptus, and **Sullivan**, a cloud based on OpenStack.

``Internet (users)``

``ssh -A login1.opensciencedatacloud.org (Adler)`` or ``ssh -A login2.opensciencedatacloud.org (Sullivan)``

``Open Science Data Cloud (VMs) -->-- NAT -->-- Internet``

``GlusterFs/CIFS (shared filesystem)``