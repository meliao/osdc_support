Public Data Commons
===========================================

Overview of Public Data Commons
--------------------------------

One of the features of the OSDC is easy access to datasets in a wide range of scientific disciplines.   
An OSDC resource allocation is not necessary to access these datasets, they are available to the general public.

The contents of the `OSDC Public Data Commons <https://www.opensciencedatacloud.org/publicdata>`_ can be 
downloaded over the internet or high performance networks such as Internet2, as well as computed over directly 
in the OSDC.  Currently, the OSDC hosts nearly 1PB of publicly available datasets in the OSDC Public Data 
Commons over glusterFS storage.

In 2015-2017, the OSDC Public Data Commons is undergoing a transition to Ceph S3 bucket storage as part of 
an `OCC initiative to provide a data commons for NOAA <http://occ-data.org/OCC_NOAA_CRADA/>`_.        

If you would like your dataset hosted in the OSDC Public Data Commons please apply at 
`https://www.opensciencedatacloud.org/keyservice/request/ <https://www.opensciencedatacloud.org/keyservice/request/>`_.   
The OSDC Resource Allocation committee reviews applications on a quarterly basis. 

.. _publicdata:

Public Dataset Locations
------------------------

The contents of the OSDC Public Data Commons are automounted on the VMs 
of some earlier versions of the OSDC software stack like OSDC Sullivan.  On Sullivan once you're logged into your VM, you can view all the publicly available datasets using ``ls /glusterfs/osdc_public_data``.   Find the subdirectories relevant to your analysis, point your VM software to them, and write your output to your home folder.  On other resources, please consult the external download instructions for each specific dataset.

.. _signpost:

Signpost ID Service
------------------------

The Signpost digital ID system balances the needs of both data archiving for persistent storage, that is, assigning identifiers to unique pieces of information, and also active computation, that is, for finding locations of data on a living system where data may be physically moved or updated. We have constructed a simple implementation of this design in a two-layer identification scheme service with a REST-like API interface.

The top layer utilizes user-defined identifiers. These are flexible, may be of any format, including Archival Resource Keys (ARKs) and Digital Object Identifiers (DOIs), and provide a layer of human readability.  The user-defined identifiers map to hashes of the identified data objects. This additionally allows for mutability by assigning different hashes and allows for reproducibility considerations.

The bottom layer utilizes hash-based identifiers. These are inflexible and identify data objects as unambiguously as possible. Hash-based identifiers guarantee immutability of identified data, allow for identification of duplicated data via hash collisions, and allow for verification upon retrieval. These map to known locations of the identified data.

By utilizing the Signpost digital identifier service, we can relocate data files from our data commons to another commons and no researcher needs to change their code.


Working with the ID Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^

After a user has received their Digital IDs, they may want to confirm integrity and download the data from their query.   Below are some example python functions from the Nexrad :ref:`Jupyter example<nexrad-example>` that review a txt file <testarks.txt> from a signpost query, check file integrity, check for a preferred repo, then download the data to the <intended_dir> defined in the download_from_arks function.  
::
	  
	 import requests
	 import urllib
	 import json
	 import hashlib
	 import os

	 #get data from txt file generated from search service
	 with open('testarks.txt', 'r') as f:
	     file_lines = f.readlines()
             for line in file_lines:
	         print line.strip() 
             id_service_arks = [line.strip().decode('utf-8-sig') for line in file_lines]


         # hash provided in signpost should match locally calculated hash
	 def confirm_hash(hash_algo, file_,actual_hash):
	     with open(file_) as f:
                 computed_hash = hash_algo(f.read()).hexdigest()
 
	     if computed_hash == actual_hash:
	         return True
	     else:
                 return False
    
         def download_from_arks(id_service_arks, intended_dir, hash_confirmation = True, pref_repo='https://griffin-objstore.opensciencedatacloud.org/'):
	     hash_algo_dict = {'md5':hashlib.md5, 'sha1':hashlib.sha1, 'sha256':hashlib.sha256}

	     for ark_id in id_service_arks:
	         signpost_url = 'https://signpost.opensciencedatacloud.org/alias/' + ark_id
		 resp = requests.get(signpost_url)         
        
	     # make JSON response into dictionary
             signpost_dict = resp.json() #json.load(resp)
             # get repository URLs
             repo_urls = signpost_dict['urls']
             for url in repo_urls:
	         print 'url = ', url
		 # if preferred repo exists, will opt for that URL
		 if pref_repo in url:
                     break
                 # otherwise, will use last url provided

	     # if you're not in Jupyter, you can use the requests library to create the files
             r = requests.get(url)
             # need file path for hash validation
             file_name = url.split('/')[-1]
             file_path = os.path.join(intended_dir, file_name)
             f = open(file_path, 'wb')
             f.write(r.content)
             f.close()
        
             # otherwise, we can run this bash command from Jupyter!
             #!sudo wget -P $intended_dir $url
       
             if hash_confirmation:
                 # get dict of hash type: hash
		 hashes = signpost_dict['hashes']
		 # iterate though list of (hash type, hash) tuples
		 for hash_tup in hashes.items():
                     # get proper hash algorithm function
                     hash_algo = hash_algo_dict[hash_tup[0]]
		     # fail if not the downloaded file has diff. hash
                     assert confirm_hash(hash_algo, file_path, hash_tup[1]), '%s hash calculated does not match hash in metadata'

         #to download, run function, make sure and have dir 'mayfly_data' created
	 download_from_arks(id_service_arks, 'mayfly_data')

.. _query_tool:

EXAMPLE:  Using the Query Tool 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The query tool allows a user to search a dataset for the parameters they wish, returning a list of Digital Ids that match the data they are looking for.  In the example below we will use the query tool to generate a list of Digital IDs relevant to the :ref:`NEXRAD analysis example<nexrad-example>`.   As part of the NOAA CRADA, the OCC committed to store all of the 2015 NEXRAD L2 archival data in the Griffin object storage.  The ID service relies on NEXRAD L2 data in OSDC Griffin and made publicly available by other NOAA BDP collaborators like Amazon Web Services. 

* Go to the `NEXRAD L2 data page <https://www.opensciencedatacloud.org/publicdata/noaa-nexrad-l2/>`_.
* Scroll to the Search Service at the bottom of the page.
* Enter '06/26/2015' in both the 'From' and 'To' fields (without the ')  
   * Required format is MM/DD/YYYY
   * NOTE: Since most weather events don't last more than a week, the "date" field will query and return a 7 day range of dates.  If you only want data from a single day, put the same date in both the 'From' and 'To' fields.
* Enter 'KARX' in the right box (without the '). 
* The ARKS for this date and station will output to a txt file equivalent to the 'mayfly_arks.txt' used in the example below. 

.. _nexrad-example:

EXAMPLE:  Analysis of NOAA's NEXRAD dataset using Signpost, Jupyter, and Py-ART
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A sample analysis of NEXRAD data is available showing how to:

* setup your work environment
* pull some data from the ID service 
* download files from the repositories the ID service references
* make multiple plots of raw reflectivity data
* filter the reflectivity data for 'bioscatter'
* animate plots 

For OSDC Griffin allocation grantees, we have there are two VMs available as public snapshots: nexrad-jupyter and nexrad-jupyter-docker that contain all the tools required to run the analysis.  Use the README.md in the VM root directory, or check the :ref:`Griffin support docs example<install-jupyter>` on how to install software and port forward Jupyter notebook to view and work locally.   

For advanced users familiar with docker commands, we recommend using the nexrad-jupyter-docker, which containerizes the different tasks, including the deployment of the notebook itself.  The containerization allows for deployment of the analysis without any of the required software installed on the VM itself.  In both snapshots, the resulting analysis is essentially the same.  

.. note:: 
   If you are using either public snapshot, all software has already been installed.  

For the larger community, the same notebooks are public in creator Ziv Dreyfuss' personal `github repository <https://github.com/zivvers/nexrad-jupyter-osdc>`_.   To simply view as a webpage, go to the `gh_pages version <http://zivvers.github.io/nexrad-jupyter-osdc/nexrad/nexrad_display_id_service_HTML_.html>`_.   

.. note::
   Not all browsers handle the animation in the Jupyter notebook demo well.   We had success using Chrome.  


ARK Key Service
------------------------

The OSDC Public Data Commons features a key service utilizing ARK codes as permanent identifiers 
to each dataset.  More information can be found here: `https://www.opensciencedatacloud.org/keyservice/ <https://www.opensciencedatacloud.org/keyservice/>`_
