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

If you would like your dataset hosted in the OSDC Public Data Commons please apply 
at `https://www.opensciencedatacloud.org/keyservice/request/ <https://www.opensciencedatacloud.org/keyservice/request/>`_.   The 
OSDC Resource Allocation committee reviews applications on a quarterly basis. 

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
*   For Hadoop resources, please review the :ref:`Hadoop Resource Guide  <hadoop>`

ARK Key Service
------------------------

The OSDC Public Data Commons features a key service utilizing ARK codes as permanent identifiers 
to each dataset.  More information can be found here: `https://www.opensciencedatacloud.org/keyservice/ <https://www.opensciencedatacloud.org/keyservice/>`_
