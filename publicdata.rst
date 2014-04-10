Public Data Commons
===========================================

Overview of Public Data Commons
--------------------------------

One of the features of the OSDC is access to already mounted datasets in a wide
range of scientific disciplines.  While an OSDC resource allocation is not 
necessary to access these datasets, it's a nice advantage.  

The contents of the `OSDC Public Data Commons <https://www.opensciencedatacloud.org/publicdata>`_ can be downloaded over the internet 
or high performance networks such as Internet2, as well as computed over directly 
on the OSDC.  Currently, the OSDC hosts nearly 1PB of publicly available datasets. 

If you have suggestions about data that should be included, please let 
us know at info@opencloudconsortium.org. 

.. _publicdata:

Public Dataset Locations
------------------------

The contents of the OSDC Public Data Commons are automounted on the VMs 
of most OSDC resources.  

Once you're logged into your VM, you can view all the publicly available datasets
using ``ls /glusterfs/osdc_public_data``.   Find the subdirectories 
relevant to your analysis, point your VM software to them, and write your output 
to your home folder.    

*	On Sullivan your home folder can be found at:  ``glusterfs/users/<username>``
*	On Atwood and PDC, you login to the VM with your username, so the dir you
	login to is your home dir.   



OCC-Y
^^^^^