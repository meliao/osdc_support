OSDC Quickstart Guide
=====================

1. Apply for and obtain a resource allocation and an account via the `OSDC application <http://www.opensciencedatacloud.org/apply>`_.   Please allow 2-3 weeks for our allocation committee to review your application.

2. Once your request has been approved and you receive a welcome email with details, log into the `main OSDC console <http://www.opensciencedatacloud.org/console>`_.

3. From the console, upload or generate a key pair to use for cloud access (:doc:`Details <ssh>`).   NOTE:  If you generate your keypair in the Tukey console, you'll probably need to run ``chmod`` to :ref:`change the permissions <ssh-linux-agent>`

4. From the console, launch a virtual machine (VM).   You can start with a plain vanilla image, or use a preexisting snapshot or image that has been already set up by your lab or another user with the required tools and software.  

.. Topic:: Coming Soon - Currently in BETA
	
		Updates to the Tukey Webconsole will allow OSDC users to more easily share descriptions of their image snapshots and the types of analysis the image is intended for.  

5. From a terminal, or a program like PuTTY in Windows, access your virtual machine by first ssh'ing into the appropriate login node with the username provided to you and then ssh'ing to your VM. If you're having trouble, please see :doc:`ssh`. The login nodes and the user to login into your VM are:

  ====================  ===================================================== ==================
  Cloud                 Login Node                             				  VM 
  ====================  ===================================================== ==================
  OSDC Adler            ``<username>@adler.opensciencedatacloud.org``         ``root@<VM.IP>`` 
  OSDC Sullivan         ``<username>@sullivan.opensciencedatacloud.org``      ``ubuntu@<VM.IP>`` 
  Bionimbus PDC         ``<username>@bionimbus-pdc.opensciencedatacloud.org`` ``<username>@<VM.IP>`` 
  OSDC Atwood (Conte)   ``<username>@atwood.opensciencedatacloud.org``        ``<username>@<VM.IP>`` 
  ====================  ===================================================== ==================

..  warning::
	
		Remember to terminate your VMs in the Tukey console when not in use to free up valuable 
		cores for other OSDC users.  Make sure if it's something you built from scratch, to 
		snapshot it first!  To learn more visit the :ref:`VM <instances>` or :ref:`FAQ/Best Practices <osdc-9>` sections.

QUICKSTART TERMINAL SESSION: SSH'ing 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Below is a screencapture of a terminal session showing the command line tools necessary to login to the OSDC Sullivan headnode and then a VM.  Feel free to copy and paste commands to your own shell, adjusting usernames and VM IPs as needed.

.. raw:: html

	<p><script type="text/javascript" src="https://asciinema.org/a/9880.js" id="asciicast-9880" async></script></p>
	
6. On Sullivan and the protected data clouds your user data is automatically mounted.  

7. Install any software packages on the VM using ``apt-get install <packagename>``.   

8. On Sullivan, you can find the datasets in the OSDC Public Data Commons at ``/glusterfs/osdc_public_data`` or your home folder at ``/glusterfs/users/<username>``.  For additional details and for other resources, please review the documents available on :ref:`Public Data Sets <publicdata>`

9. Follow the Tutorial below for a hands-on demonstration of how to use the OSDC.

10.  When you have technical issues, please review this support guide.   If you are unable to resolve them, contact us at `support@opensciencedatacloud.org <support@opensciencedatacloud.org>`_.   A member of our support team will review and contact you as soon as possible. 

.. _EO-1Tutorial:

OSDC EO-1 Quick Start Tutorial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Now we'll take you step by step through a demo using NASA's Earth Observing-1 dataset. 
In this tutorial, we will show you how to use OSDC to visualize and perform a simple 
example analysis of NASA satellite imagery data.   You will perform many tasks common to
using the OSDC during this demo like launching an instance, ssh'ing, in addition to those specific
to analysis.  

.. sidebar:: About the Data

	NASA's Earth Observing-1 satellite (EO-1) was launched in 2000 for the purpose of 
	studying new technologies in remote earth imaging. On the OSDC, we host data from 
	EO-1's two primary scientific instruments, the Hyperion imaging spectrometer and the 
	Advanced Land Image (ALI). In this tutorial we will be working with ALI data.

	The ALI instrument acquires data in 9 different wavelength bands from 0.48 - 2.35 micron
	with 30-meter resolution plus a panchromatic band with higher 10-meter spatial resolution.  
	The standard 'scene' (image) size projected on the Earth's surface equates to 37 km x 42 km 
	(width x length).  Hyperion has similar spatial resolution but higher spectral resolution, 
	observing in 242 band channels from 0.357 - 2.576 micron with 10-nm bandwidth. 
	Hyperion scenes have a smaller standard footprint width of 7.7 km.

	EO-1 Level 0 scenes (raw data) are received daily from NASA and processed by NASA on the 
	OSDC to create various Level 1 data.  We will use here the Level 1Gst scenes, 
	radiometrically corrected, resampled for geometric correction, and registered to a 
	geographic map projection. These data are stored in GeoTiff format, one GeoTiff for each wavelength 
	band, giving the corrected radiance value recorded at each pixel. 

Here we will show you how to use Python to 

*  create png false-color images from GeoTiff data,
*  use a machine algorithm to classify each pixel of a scene as desert, water, cloud, or vegetation,
*  view GeoTiffs and save the results of your classification as an image.

Launch the OSDC EO-1 Instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In the console, under 'Images and Snapshots', scroll down to find the section labeled 'All Snapshots'.  Here's you'll want to find 
and launch the snapshot called 'OSDC_DatasetExplorer_EO1'.   Tiny or small flavor will be suitable for our purposes.  

When you ssh in to both the login node and the instance, make sure and add both the "A" and the "X" flags.  The A is for key forwarding, the X
is for X11 forwarding.  IE:  ``ssh -AX <username>@sullivan.opensciencedatacloud.org`` and then ``ssh -AX ubuntu@<INSTANCE.IP>``.  If you're doing a lot of 
GUI work like looking at plots and images, you'll want to use this X flag often.

Viewing a GeoTiff
~~~~~~~~~~~~~~~~~
We will take a look at an example ALI GeoTiff from band 3, covering 0.45 - 0.515 micron. 
Our data resides in the /glusterfs/osdc_public_data/eo1 directory.  In the terminal, type or copy:

``python viewGeoTiff.py /glusterfs/osdc_public_data/eo1/ali_l1g/2014/029/EO1A1930292014029110PZ_ALI_L1G/EO1A1930292014029110PZ_B03_L1T.TIF``

Making an RGB Image
~~~~~~~~~~~~~~~~~~~
Here we will create an RGB image from three bands of an individual ALI scene. 
We will use the makeRGB.py script to look at a scene observed on the 29th 
day of 2014 and save it as a png image.  To make the image a little brighter,
we tell the script to scale each color up by a factor of 2.

In the terminal, type or copy in:

``python makeRGB.py 2014 029 EO1A1930292014029110PZ italy.png 2``

To download this image to your local machine for viewing is a two-step process.
First, move the file to your gluster user directory on Sullivan
by typing the following into your VM terminal:

``mv italy.png /glusterfs/users/USERNAME/``

Then, in the terminal on your local machine, download the file into the preferred directory:

``scp USERNAME@sullivan.opensciencedatacloud.org:~/italy.png .``

Now take a look at your picture using your favorite image viewer.
Looks like a nice spot to run our classifier. This is a section of the Italian coast near Pisa.
 
Classifying the Image
~~~~~~~~~~~~~~~~~~~~~
We will run our classifier see if it can identify which sections of the scene are clouds, 
water, desert, or vegetation.  The classifier uses a support vector machine (SVM) 
from Python's scikit-learn module to fit a model
to the training set from Hyperion data we have provided in 'FourClassTrainingSet.txt'. 
This classifier uses the ratios of ALI bands 3:7 and 4:8.
The file trainingSpectra.png shows a plot of the average reflectance spectra from Hyperion 
for each class in the training set.  Shaded grey areas show the wavelength coverage of
ALI bands, which are used by the classifier described.

You can run the classifier with the following command:

``python classify.py 2014 029 EO1A1930292014029110PZ italyClassified.tif``

It will take about 10 minutes to run, so go get a snack or some coffee. You 
can also look at the classified GeoTiff we have provided using the above procedure.

.. Topic:: INTERMISSION - Project Matsu
	
		This demonstration comes from analysis demonstrated by one of our OSDC 
		projects called `Project Matsu <http://matsu.opensciencedatacloud.org/>`_.  Project Matsu is a collaboration between 
		NASA and the Open Cloud Consortium to develop open source technology for cloud-based processing of 
		satellite imagery to support the earth sciences and disaster relief. 

Viewing the Results
~~~~~~~~~~~~~~~~~~~
Let's take a look at the GeoTiff created. Run viewClassifiedTiff.py on the file
made by the classification:

``python viewClassifiedTiff.py italyClassified.tif italyClassified.png``

You can download italyClassified.png to your local machine using the instructions 
above in 'Making an RGB image.' The classified scene has a white pixel where the classifier identified clouds, 
blue for water, brown for desert, and green for vegetation.  Using the `USGS EarthExplorer webpage <http://earthexplorer.usgs.gov/>`_ 
you can retrieve the scene IDs and dates for scenes all over the world and classify them. Have fun! 

Cleaning up
~~~~~~~~~~~
Once you have completed this demo, exit out of the VM and the login node, enter the console and be
sure to terminate your VM.  


