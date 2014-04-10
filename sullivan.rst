Resource Guide - Sullivan (Public Cloud)
===========================================

Overview
-----------------



Command Line Tools
------------------

Floating IPs
------------

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




Sample Images
--------------