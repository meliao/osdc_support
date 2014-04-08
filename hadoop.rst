Resource Guide - OCC-Y and Skidmore (Hadoop)
============================================

Overview of OSDC Hadoop Resources
----------------------------------

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

