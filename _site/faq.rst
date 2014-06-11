Frequently Asked Questions (FAQ) / Best Practices
=====================================================
General Questions
-----------------------------------------------------
- :ref:`osdc-1`
- :ref:`osdc-2`
- :ref:`osdc-3`
- :ref:`osdc-4`
- :ref:`osdc-5`
- :ref:`osdc-6`
- :ref:`osdc-7`
- :ref:`osdc-8`
- :ref:`osdc-9`
- :ref:`osdc-10`
- :ref:`osdc-11`

Protected Data Clouds
-----------------------------------------------------
- :ref:`pdc-1`
- :ref:`pdc-2`
- :ref:`pdc-3`
- :ref:`pdc-4`
- :ref:`pdc-5`
- :ref:`pdc-6`
- :ref:`pdc-7`
- :ref:`pdc-8`

General Questions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. _osdc-1:

I can't SSH into the login node or VM?
******************************************************
  Please see :doc:`ssh`. 

.. _osdc-2:

Why don't VMs have external IPs?
******************************************************
  We do not have enough IPs to assign every VM its own. Typically, for development we recommend either using ssh port forwarding or tsocks to access the VMs directly. If you need an external IP for a production purpose let us know and we'll try to accommodate the request.

.. _osdc-3:

What is GlusterFS?
******************************************************
  GlusterFS is a scalable, distributed file system that we use on our clouds to provide file level access to data. Each cloud has it's own GlusterFS store that is visible from all nodes and VMs. Additionally, the GlusterFS store that contains the OSDC public datasets is readable from all locations.

.. _osdc-4:

Why are there quotas?
******************************************************
  We are providing a shared community resource so there are default quotas for storage and number of cores on each cloud for new users. If you require more resources for a specific project we can work with you to increase these :ref:`quotas <metering>`.

.. _osdc-5:

How do I contribute a new public data set?
******************************************************
  Please `contact us <support@opensciencedatacloud.org>`_ and we can set up a folder where you place your public data for the community to use.

.. _osdc-6:

What is the fastest way to transfer data to/from the cloud?
************************************************************************************************************
  We provide a tool called `UDR <https://github.com/LabAdvComp/UDR>`_ that works just like rsync but utilizes a high performance network protocol called `UDT <http://udt.sourceforge.net/>`_. It is freely available on our `GitHub <https://github.com/LabAdvComp/UDR>`_ page.

.. _osdc-7:

How do I share data with just my collaborators?
******************************************************
  `Contact us <support@opensciencedatacloud.org>`_ and we can set up project groups that you can use to share data only with other users in that group. This is done by using Linux ACLs.

.. _osdc-8:

What's the best approach to setup a new pipeline and install packages?
************************************************************************************************************
  Depending on your pipeline the software may need to be installed on all of the nodes 
  and will definitely need to be installed on the compute nodes. A good way to do this 
  is to start a VM and install the packages you need using `apt <https://help.ubuntu.com/community/AptGet/Howto>`_ 
  or under /usr/local/bin and then creating a :ref:`snapshot <snapshot>` of that VM. Then select 
  that image when launching your cluster for both the headnode and compute nodes.

.. _osdc-9:

What should I know about snapshots?
******************************************************
  You can go to the :ref:`snapshot <snapshot>` section of our instance page to learn more, 
  but in short, snapshots are ways to share and save packages you've installed on instances for later use.
  We're currently working on setting up methods for users to add additional metadata so that 
  you and other OSDC users can understand what types of packages are installed and what type of analysis
  was conducted with said VM.
  
.. _osdc-10:

Why is it important that I terminate my VMs?
******************************************************
  The OSDC is a publicly shared resource, and supports a wide variety of researchers from a number of 
  different scientific disciplines.   When you have instances that are not in use, but are not terminated, 
  those cores are still reserved for your idling instances.   That prevents other researchers from 
  using those cores.   Note:   Suspending images still keeps those cores reserved and will continue to be
  counted in :ref:`metering <metering>`.  Terminating images not in use is definitely the best practice.
  
.. _osdc-11:

Who should I contact with further questions?
******************************************************
  Please email support@opensciencedatacloud.org for the fastest response.



Protected Data Clouds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. _pdc-1:

What are protected data clouds?
******************************************************
  Protected data clouds provide a secure cloud computing environment to store and analyze sensitive data such as human genomic data. We operate two protected data clouds. One is the "Bionimbus Protected Data Cloud", which is a partnership with `The Cancer Genome Atlas <http://cancergenome.nih.gov/>`_ to store the Level 1 sequence data and provide computational resources that have direct access to this data. The other is "OSDC Atwood" which is the same architecture that is being used by several projects, including the `Conte Center for Computational Neuropsychiactric Genomics <http://www.contechicago.org/>`_.

.. _pdc-2:

How do I gain access to the protected data clouds?
******************************************************
  Bionimbus-PDC hosts protected Level 1 data from TCGA and so you must have dbGaP authorized access for the TCGA Level 1 and 2 data. The system uses your eRA commons to check against a list from NIH of authorized users for the TCGA protected data. This list is updated daily. You can apply for access via the `TCGA dbGaP project site <http://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs000178.v8.p7>`_. Once you have dbGaP access then you just need to provide us your information and eRA commons user name on the `Bionimbus-PDC application page <https://bionimbus-pdc.opensciencedatacloud.org/apply/>`_.

  If you are part of a project hosted on OSDC Atwood, please fill out the main `OSDC application page <https://www.opensciencedatacloud.org/apply/>`_ and select "OSDC Atwood".

.. _pdc-3:

I am a PI and have dbGaP access, can I share this access with others in my group?
************************************************************************************************************
 There is now a "downloaders" role in dbGaP for this purpose. Information on how to set this up can be found `here <http://www.ncbi.nlm.nih.gov/books/NBK36439/#Download.i_am_a_principal_investigator>`_.

.. _pdc-4:

What is the advantage of using PDCs instead of downloading the data locally?
************************************************************************************************************
- FISMA certified architecture so you don't have to worry about security
- Virtual machines have immediate access to large datasets, such as TCGA, which is currently > 500 TB and projected to grow to > 2 PB. 
- Ability to configure and save virtual machines
- Scale up or down the number of virtual machines running based on your current needs 

.. _pdc-5:

Why is there no root access on the PDC?
******************************************************
  As part of the security certification process, the decision was made to not allow full root access on the VMs. However, there is sudo access to install packages with apt and if you require privileged access we will gladly work with you to provide the access you need. 

.. _pdc-6:

Why is http access blocked on the VMs?
******************************************************
  All the VMs use an http_proxy that filters content based on a whitelist we maintain. If you need access to a specific resource, please `contact us <support@opensciencedatacloud.org>`_ and we can easily add it to the whitelist.

.. _pdc-7:

How do I get access to TCGA data required to use the PDC?
************************************************************************************************************
  You can learn more about how to get the proper credentials to use the PDC `by reviewing this FAQ <https://bionimbus-pdc.opensciencedatacloud.org/static/BionimbusAccessFAQ.pdf>`_.
  
.. _pdc-8:

How do I gain access to the Conte resource?
******************************************************
  The resource we maintain for Conte is called Atwood.   If you are part of a project hosted by 
  Conte, please fill out the main OSDC application page and select Protected OpenStack Cloud - 
  then in the Project description please note the need for access to the Atwood resource, and the 
  protected Conte datasets to which you're requesting access.
  
I've reviewed the available documentation, but still have an issue.  What now?
--------------------------------------------------------------------------------------

Contact us at `support@opensciencedatacloud.org <support@opensciencedatacloud.org>`_.   This will create a ticket we can track and a member of our support team will review and contact you as soon as possible. 