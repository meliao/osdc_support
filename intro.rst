Introduction to the OSDC
===========================================

Overview
^^^^^^^^

The `Open Science Data Cloud <https://www.opensciencedatacloud.org>`_
(OSDC) is a distributed, cloud-based infrastructure for managing,
analyzing, archiving and sharing scientific data sets.   

The OSDC operates two major public compute clouds named Sullivan and Griffin 
and a few separate storage resources.  Sullivan and Griffin are both 
`OpenStack <http://www.openstack.org/>`_ based clouds.   
Sullivan is older and uses GlusterFS for storage, while Griffin utilizes 
ephemeral storage in VMs for scratch space and S3 storage for a longer term
shared term solution. 

Root hosts about 1 PB of `public data sets 
<http://www.opensciencedatacloud.org/publicdata>`_ and is 
accessible from all OSDC resources.

.. NOTE:: OSDC resources are generally named after prominent Chicago Architects ie:  Griffin = Marion Griffin, Sullivan = Louis Sullivan; etc...

The OSDC also operates protected clouds like the `Bionimbus Protected Data Cloud 
<https://bionimbus-pdc.opensciencedatacloud.org>`_ to provide platforms 
for users to compute over large protected human genomic datasets. We generally refer to these as "PDCs".  

.. NOTE::   OSDC resources that are not specifically labeled as protected clouds are not designed to be used for the analysis or storage of any Protected Health Information (PHI) or other sensitive data.

Each uses either a distributed `Gluster File System <http://www.gluster.org/>`_ or object storage and ephemeral storage in VMs 
where users can store their data and access it from all of their VMs.  

The OSDC manages two Hadoop clusters, OCC-Y and Skidmore, for select projects. 

.. _allocations:

OSDC Resource Allocations
^^^^^^^^^^^^^^^^^^^^^^^^^

The Open Science Data Cloud ecosystem has a very active community of BETA users 
and demand for OSDC services are growing. To help keep up with the demand 
for computing resources, we review active resource allocations on a quarterly 
basis and make sure that allocations are adequately distributed among 
projects based on scientific merit and/or established partnership status 
with the OSDC. This is done through a process of surveys to active grantees 
of OSDC resource allocations.

Resource allocation periods are generally for 3 months at a time and begin 
on January 1, April 1, July 1, October 1. All incoming applications 
for resources will be reviewed near one of these terms and are due on the 
15th of the month prior (e.g., December 15th for the allocation period 
starting January 1st). During the survey process, a resource allocation 
extension can be requested if your research is not yet complete.

A general resource allocation on the OSDC can be requested through 
the `application process <https://www.opensciencedatacloud.org/apply>`_.   
Special protected resources like the `Bionimbus-PDC 
<https://bionimbus-pdc.opensciencedatacloud.org/>`_ have their own 
separate application process. 

.. NOTE:: The OSDC console uses federated login. If your organization is on the list of 
	`InCommons members <https://incommon.org/federation/info/all-orgs.html>`_, 
	`UK Federation members <http://www.ukfederation.org.uk/content/Documents/MemberList>`_, 
	or `CANARIE members <http://www.canarie.ca/en/about/partners/members>`_, 
	please apply using those credentials.   The Bionimbus PDC also uses `eRA Commons <https://commons.era.nih.gov/>`_ 
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
*       Do not share private ssh keys or login information - one user per resource allocation.
*	:ref:`Cite  <cite>` the OSDC in any papers describing research
*	Regularly respond to quarterly OSDC allocation surveys
*       Submit tickets to the OSDC support ticketing system when encountering technical issues not covered by the OSDC support documentation.

.. _pdcs:

Protected Data Resources
^^^^^^^^^^^^^^^^^^^^^^^^

Some protected `OSDC resources <https://www.opensciencedatacloud.org/systems/>`_ or 
Protected Data Clouds (PDC) require special certification or legal agreements to 
be shared with our allocation team before a researcher gains access.   Ensuring that PDC
allocation receipients are trained in handling sensitive data is an important step 
in keeping these resources secure and compliant. 

.. warning::   OSDC resources that are not specifically labeled as protected clouds are not designed to be used for the analysis or storage of any Protected Health Information (PHI) or other sensitive data.

.. _citi:

PDC - Handling Sensitive Data Training:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
PDC resource allocation recipients are required to provide copies of the standard 
training and organizational approvals that all projects that use protected health 
information and other sensitive data must have.  

The Protected Data Cloud provides a computing environment designed to 
support research with protected health information and other sensitive data, but 
the computing environment is only one component of what is required to properly 
secure protected health information or other sensitive data.   Another important 
requirement is that those using a computing infrastructure like the PDC have 
proper training and have received from their institution all the required approvals 
for working in secure environments.

We require the following documentation:
 
1) For each project, that the PI of the project send us a copy of the IRB approved protocol for the study using the Protected Data Cloud or a letter showing the study is exempt from needing an IRB Protocol.   The IRB Protocol or Exemption should be from the home institution of the PI for the project.   For new protocols or renewals of protocols, please refer explicitly to the PDC environment.
2) For each researcher with a Protected Data Cloud account, a certificate proving that the researcher has completed CITI training appropriate for working in secure environments.  For international researchers, we can accept NIH security training as an alternative.  A copy of a certificate indicating that you have completed the required training will be requested each year.  

.. NOTE::   We recognize that some institutions do not support CITI training.   If CITI training is not available at your institution, we can review and accept other forms of certification indicating proof of training handling PHI on a case by case basis.   For interational researchers, we can accept NIH security training as an alternative.
 
CITI Training:
~~~~~~~~~~~~~~~~~~~~ 
Please complete the following four modules from the Human Subjects Research - Biomedical Modules available through CITI training.  

* Human Subjects Research – Biomedical (Biomed) Modules
   * Basics of Health Privacy
   * Health Privacy Issues for Researchers
   * Basics of Information Security, Part 1
   * Basics of Information Security, Part 2
* Suggested Modules
   * Research and HIPAA Privacy Protections
   * Protecting Your Computer

Here are some details (NOTE:  users at different institutions, and at different depts within an institution may not see these exact messages):

* Go to `CITI home page <http://www.citiprogram.org>`_
* If you do not already have a CITI account, go to "Create an account" --> Register and select your Organization Affiliation.  Be sure to select your home institution as your “Participating Institution,” select a username/password, and fill out all other necessary information requested in registration. 
* Once this is completed you will be required to complete 4 enrollment questions. This will determine the modules you will need to complete.  
* Please make the following choices:
    * Would you like to take the Conflict of Interest Course? - "Yes"
    * I would like to complete the optional Good Clinical Practices (GCP)
    * Select your Division at your Institution.
    * Researchers involved in protocols need to complete CITI Basic/Refresher - "Need to Enroll for Research Staff"
    * Responsible and Ethical Conduct of Research course - "Yes"
* Once complete, send your certificate to accounts@occ-data.org   

NIH Training:
~~~~~~~~~~~~~~~~~~~~ 

For international collaborators unable to complete CITI training that need access to the PDC, we can accept proof of completion of the `"Entire NIH Information and Security Awareness Course" <http://irtsectraining.nih.gov/publicUser.aspx>`_ offered by the NIH.   

VIDEO: OSDC Demo
^^^^^^^^^^^^^^^^

You can learn more about the OSDC in general by watching this webconference demonstration on the OSDC and how to use the Sullivan resource. 

.. raw:: html

        <p><object width="480" height="385"><param name="movie"
        value="https://www.youtube.com/v/XNLhKS8VhVE?version=3&amp;hl=en_US&amp;rel=0&hd=1"></param><param
        name="allowFullScreen" value="true"></param><param
        name="allowscriptaccess" value="always"></param><embed
        src="https://www.youtube.com/v/XNLhKS8VhVE?version=3&amp;hl=en_US&amp;rel=0&hd=1"
        type="application/x-shockwave-flash" allowscriptaccess="always"
        allowfullscreen="true" width="480"
        height="385"></embed></object></p>



