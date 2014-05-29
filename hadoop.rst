Hadoop Resource Guide - OCC-Y and Skidmore
============================================

.. _hadoop:

OCC-Y
-----

Connecting to the OCC-Y Hadoop cluster:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*  You must log in to one of the two servers available.  All files not stored in HDFS should be stored in 
	your home directory.  Your home directory is shared between the two login nodes below.

	*  If you are on a internet2/starlight accessible network you ssh into occy-starlight.opensciencedatacloud.org
		``ssh -A username@occy-starlight.opensciencedatacloud.org``
	*  If you are using normal internet1 then you need to use  occy.opensciencedatacloud.org .
		``ssh -A username@occy.opensciencedatacloud.org``
 
*  Once logged in you may issue commands as needed. 

	*  example of seeing files in your hdfs directory:
		``hadoop dfs -ls  /user/$USER``
	*  example of launching a job:
		``hadoop jar /opt/hadoop/hadoop-examples-0.20.203.0.jar wordcount /user/$USER/in/ /user/$USER/out/``



Loading and Accessing Data on the OCC Y Hadoop Cluster:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Before Hadoop jobs can be run over data it needs to be loaded into the Hadoop filesystem HDFS. Loading data in 
   and out of the Hadoop cloud is done in two stages.  First data must be loaded into your home directory.  Then from 
   there it is loaded into HDFS.  All users have a home directory inside hdfs at /user/$USER.  Please note that the 
   HDFS filesystem is completely separate from the usual linux filesystem.  Please only reference /user/$USER inside 
   HDFS.  Please use scp, sftp, rsync, or UDR to load data in and out of OCC Y.			

#. Once the files exist on your home directory they can be loaded into your HDFS directory  at /user/$USER with 
   the `hadoop fs/dfs commands <http://hadoop.apache.org/common/docs/r0.20.0/hdfs_shell.html>`_.

	* Copy files into hadoop 
		``hadoop dfs -moveFromLocal /home/$USER/source_on_local_disk /user/$USER/target_in_hdfs``
	* Copy files out of hadoop
		``hadoop dfs -CopyToLocal /user/$USER/source_in_hdfs /home/$USER/target_on_local_disk``


Skidmore
--------

Connecting to the Skidmore cluster:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1.	ssh, forwarding your ssh key, into skidmore.opensciencedatacloud.
 
	``ssh -A username@skidmore.opensciencedatacloud.org`` 
	``ssh -A username@starlight``

2.	Once logged into skidmore.opensciencedatacloud.org, you may issue commands as needed. 

	* example of seeing files in your hdfs directory:
		``hdfs dfs -ls  /user/$USER``
	* example of launching a job:
		``hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-examples-2.0.0-cdh4.4.0.jar wordcount /user/$USER/in/ /user/$USER/out/``

Loading and Accessing Data on the Skidmore Hadoop Cluster:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1.	Before Hadoop jobs can be run over data it needs to be loaded into the Hadoop filesystem HDFS. Loading data in and out of the Hadoop cloud is done in two stages.  First data must be loaded into your home directory on the starlight server.  Then from there it is loaded into HDFS.  All users have a home directory inside hdfs at /user/$USER.  Please note that the HDFS filesystem is completely separate from the ussual linux filesystem.  Please only reference /user/$USER inside HDFS.
			
2.	Once the files exist on your home directory they can be loaded into your HDFS directory  at /user/$USER with the hdfs or hadoop fs/dfs commands (http://hadoop.apache.org/common/docs/r0.20.0/hdfs_shell.html)

	* Copy files into hadoop 
	  ``hdfs dfs -moveFromLocal /home/$USER/source_on_local_disk /user/$USER/target_in_hdfs``
	* Copy files out of hadoop
	  ``hdfs dfs -CopyToLocal /user/$USER/source_in_hdfs /home/$USER/target_on_local_disk``
   	

Manipulating files in HDFS
--------------------------
* the "hadoop fs" contains all the commands you need to manipulate files in HDFS.  run ``hadoop fs -help`` to see all options

Loading files into HDFS
^^^^^^^^^^^^^^^^^^^^^^^
In order to run hadoop jobs over your files, you need to copy them into Hadoop's special filesystem called HDFS.  The filename used are just examples, substitute them with your own.  Also please do not attempt to write/store files outside your designated home directories.

* scp/sftp/rsync/UDR the files into your home directory on the linux filesystem. 
    ``scp -r ./myfiles_in $USERNAME@$CLOUD.opensciencedatacloud.org:``

* Load the files into HDFS at /user/$USERNAME. 
    ``hadoop fs -moveFromLocal ~/myfiles_in /user/$USERNAME/in``


Pulling files out of HDFS
^^^^^^^^^^^^^^^^^^^^^^^^^
In order to copy the file out of the Hadoop clouds, you first need to copy the file off of HDFS to your home dirctory.

* Copy the files out of HDFS to your home directoy. 
    ``hadoop fs -CopyToLocal /user/$USERNAME/out ~/myfiles_out``

* scp/sftp/rsync/UDR the files out of your home directory on the linux filesystem.  
    ``scp -r $USERNAME@$CLOUD.opensciencedatacloud.org:myfiles_out ./``

Launching jobs in Hadoop
------------------------
Hadoop jobs are custom Java application that you have written or acquired.  Ideally you would store these in your home directory on the local filesystem.  Most jobs will be of the form ``hadoop jar YOURJARFILE.jar Option1 option2.....OptionN``.  Since every program will have its own syntax we will only show the default example wordcount.  Please consult the source of your hadoop program or review the code for proper syntax.
We assume /user/$USERNAME/in has been prepopulated as described above and the /user/$USERNAME/out does not exist.  The wordcount example is part of the hadoop-examples.jar provided by apache,  the listed version number may change depending on the cloud and hadoop version.

* WordCount:  
    ``hadoop jar /opt/hadoop/hadoop-examples-0.20.203.0.jar wordcount /user/$USERNAME/in/ /user/$USERNAME/out/``

