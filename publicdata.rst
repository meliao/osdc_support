Public Data Commons
===========================================

Overview of Public Data Commons
--------------------------------

One of the features of the OSDC is easy access to datasets in a wide range of scientific disciplines.  While 
an OSDC resource allocation is not necessary to access these datasets, an allocation allows a researcher to 
conduct analysis without the hassle of transfering and managing this data. 

The contents of the `OSDC Public Data Commons <https://www.opensciencedatacloud.org/publicdata>`_ can be 
downloaded over the internet or high performance networks such as Internet2, as well as computed over directly 
on the OSDC.  Currently, the OSDC hosts nearly 1PB of publicly available datasets in the OSDC Public Data 
Commons over glusterFS storage.

In 2015-2016, the OSDC Public Data Commons is undergoing a transition to Ceph S3 bucket storage as part of 
an `OCC initiative to provide a data commons for NOAA <http://occ-data.org/OCC_NOAA_CRADA/>`_.        

If you would like your dataset hosted in the OSDC Public Data Commons please apply at 
`https://www.opensciencedatacloud.org/keyservice/request/ <https://www.opensciencedatacloud.org/keyservice/request/>`_.   
The OSDC Resource Allocation committee reviews applications on a quarterly basis. 

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
*       For Hadoop resources, please review the :ref:`Hadoop Resource Guide  <hadoop>`
*       For Griffin, please review the :ref:`Griffin Resource Guide  <griffin>`

Signpost ID Service
------------------------

COMING SOON

Working with the ID Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^

After a user has received their ARKs, they may want to confirm integrity and download the data from their query.   Below are some example functions from the Nexrad Jupyter example {INSERT LINK} that review a txt file <id_service_arks> from a signpost query, check file integrity, check for a preferred repo, then download the data to the <intended_dir> defined in the download_from_arks function.  
::
   :linenos:
	  
	  import requests
          # hash provided in signpost should match locally calculated hash
	      def confirm_hash(hash_algo, file_,actual_hash):
	          with open(file_) as f:
                      computed_hash = hash_algo(f.read()).hexdigest()
 
	          if computed_hash == actual_hash:
	              return True
		  else:
                      return False
    
	  # download, validate(optional) NEXRAD L2 data 
	  def download_from_arks(id_service_arks, intended_dir, hash_confirmation = True, pref_repo='https://griffin-objstore.opensciencedatacloud.org/'):
	      hash_algo_dict = {'md5':hashlib.md5, 'sha1':hashlib.sha1, 'sha256':hashlib.sha256}
    
	      for ark_id in id_service_arks:
                  signpost_url = 'https://signpost.opensciencedatacloud.org/alias/' + ark_id
		  resp = requests.get(signpost_url,
                           proxies={'http':'http://cloud-proxy:3128','https':'http://cloud-proxy:3128'} 
                           )
        
	      # make JSON response into dictionary
              signpost_dict = resp.json()
        
              # get repository URLs
              repo_urls = data_url = signpost_dict['urls']
       
              for url in repo_urls:
	          # if preferred repo exists, will opt for that URL
		  if pref_repo in url:
		      break
                  # otherwise, will use last url provided
        

              # need file path for hash validation
              file_name = url.split('/')[-1]
              file_path = os.path.join(intended_dir, file_name)

              # wow! if you're in Jupyter we can do this from a single bash command.
              # !sudo wget -P $intended_dir $url

              # if you're not in Jupyter, you can use the requests library to create the files
	      r = requests.get(url)
	      f = open(file_path, 'wb')
	      f.write(r.content)
	      f.close()
        
              if hash_confirmation:
                  # get dict of hash type: hash
		  hashes = signpost_dict['hashes']
		  # iterate though list of (hash type, hash) tuples
		  for hash_tup in hashes.items():
                      # get proper hash algorithm function
                      hash_algo = hash_algo_dict[hash_tup[0]]
                      # fail if not the downloaded file has diff. hash
                      assert confirm_hash(hash_algo, file_path, hash_tup[1]), '%s hash calculated does not match hash in metadata' % file_path    


ARK Key Service
------------------------

The OSDC Public Data Commons features a key service utilizing ARK codes as permanent identifiers 
to each dataset.  More information can be found here: `https://www.opensciencedatacloud.org/keyservice/ <https://www.opensciencedatacloud.org/keyservice/>`_
