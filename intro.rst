Introduction to the OSDC
===========================================

Overview
^^^^^^^^

The `Open Science Data Cloud <https://www.opensciencedatacloud.org>`_
(OSDC) is a distributed, cloud-based infrastructure for managing,
analyzing, archiving and sharing scientific data sets.   

The OSDC operates one major public compute cloud named Sullivan and one storage cloud named Root. 
Sullivan is an `OpenStack <http://www.openstack.org/>`_ based cloud and the default cloud for
new users. Root hosts about 1 PB of `public data sets <http://www.opensciencedatacloud.org/publicdata>`_ and is accessible from all OSDC resources.

.. NOTE:: OSDC resources are generally named after prominent Chicago Architects ie:  Sullivan = Louis Sullivan;
	Atwood = Charles Atwood,  etc...

The OSDC also operates several protected clouds to provide platforms for users to compute over large protected human genomic datasets. These include the `Bionimbus Protected Data Cloud <https://bionimbus-pdc.opensciencedatacloud.org>`_ and the `Atwood (or Conte) Cloud <http://www.contechicago.org/conte-cores/core-b>`_.

Each cloud has a distributed `Gluster File System <http://www.gluster.org/>`_ where users can store their data and access it from all of their VMs.

The OSDC manages two Hadoop clusters, OCC-Y and Skidmore, for select projects. 


OSDC Resource Allocations
^^^^^^^^^^^^^^^^^^^^^^^^^

The Open Science Data Cloud has a very active community of BETA users and demand for OSDC services are growing. To 
help keep up with the demand for computing resources, we review active resource allocations on a quarterly basis 
and make sure that allocations are adequately distributed among projects based on scientific merit and/or 
established partnership status with the OSDC. This is done through a process of surveys to active recipients 
of OSDC resource allocations.

Resource allocation periods are generally for 3 months at a time and begin on January 1, April 1, July 1, October 1. 
All incoming applications for resources will be reviewed near one of these terms and are due on the 15th of the 
month prior (e.g., December 15th for the allocation period starting January 1st). During the survey process, a 
resource allocation extension can be requested if your research is not yet complete.

A general resource allocation on the OSDC can be requested through the `application 
process <https://www.opensciencedatacloud.org/apply>`_.   Special protected resources
like the `Bionimbus-PDC <https://bionimbus-pdc.opensciencedatacloud.org/>`_ have their own 
separate application process. 

.. NOTE:: The OSDC console uses federated login. If your organization is on the list of 
	`InCommons members <https://incommon.org/federation/info/all-orgs.html>`_, 
	`UK Federation members <http://www.ukfederation.org.uk/content/Documents/MemberList>`_, 
	or `CANARIE members <http://www.canarie.ca/en/about/partners/members>`_, 
	please apply using those credentials.   The Bionimbus PDC uses `eRA Commons <https://commons.era.nih.gov/>`_ 
	for secure authentication. 

Once you've applied, you'll hear back from us shortly with any additional follow up 
questions regarding your request for resources.   Your request will then be considered 
by our resource allocation committee during the allocation review period.  Due to space 
limitations, not all requests will be approved.

Once your application is approved and your account is created, you'll receive an email 
welcome with instructions for getting started.   

Resource allocations will be reviewed periodically to make sure recipients are making
progress with their research questions and making appropriate use of their allocations. 
This will be done via surveys and direct emails.   

Recipients of OSDC resource allocations are expected to

*	Make appropriate use of OSDC resources and use :ref:`good social behavior  <metering>`.
*	:ref:`Cite  <cite>` the OSDC in any papers describing research
*	Let us know about papers published when asked in surveys

VIDEO: OSDC Demo
^^^^^^^^^^^^^^^^

You can learn more about the OSDC in general by watching this OSDC Demo webconference. 

.. raw:: html

        <p><object width="480" height="385"><param name="movie"
        value="https://www.youtube.com/v/XNLhKS8VhVE?version=3&amp;hl=en_US&amp;rel=0&hd=1"></param><param
        name="allowFullScreen" value="true"></param><param
        name="allowscriptaccess" value="always"></param><embed
        src="https://www.youtube.com/v/XNLhKS8VhVE?version=3&amp;hl=en_US&amp;rel=0&hd=1"
        type="application/x-shockwave-flash" allowscriptaccess="always"
        allowfullscreen="true" width="480"
        height="385"></embed></object></p>



