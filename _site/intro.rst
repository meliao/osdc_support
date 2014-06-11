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
questions regarding your request for resources.   Your  request will then be considered 
by our resource allocation committee.  Please allow 2-3 weeks for review.   Due to space 
limitations, not all requests will be approved.

Once your application is approved and your account is created, you'll receive an email 
welcome with instructions for getting started.   

Resource allocations will be reviewed periodically to make sure recipients are making
progress with their research questions and making appropriate use of their allocations. 
This will be done via surveys and direct emails.   

Recipients of OSDC resource allocations are expected to:

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



