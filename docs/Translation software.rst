#############
Translation software documentation 
#############

This document describes in detail how the translation is done from the raw data files Coursera and edX provide and how to run the 
software and different assumptions and heuristics used during the translation.Currently translation software is available for 
three different platforms-edX, OpenedX and Coursera. 


edx
===

**********************
Files from edX for a course called <course name>
**********************

To process the tracking log files and get into MOOCdb format, we provide the following detailed instructions. First step is to install a machine with 
required packages and disk space. 

**************************************
Step 1: Installing the required packages or downloading a VM 
**************************************

 * Option 1: To run the software you may want to download a VM from Amazon cloud. 
      This virtual machine image comes with all 
      packages installed required to run the MOOCdb pipeline. To get a link to the image and request the pem file, please email
      kalyan@csail.mit.edu. 
  
        * When instantiating this virtual machine on Amazon or locally, please provision the disk space (hard disk) 
          such that there is atlease three times the size of the decrypted- uncompressed file size of the tracking logs.
 * Option 2: Install all the packages on your local machine 
        The following packages are required on a MOOCdb machine 
        
        #. Install ``Unidecode`` package available at 
        
        #. Install ``ijson`` package available at 
        
        #. Install ``python-setuptools``
        
        #. Install ``pip`` using 
        
              ``sudo easy_install pip``

        #. Install ``pandas`` 
                * Make sure your Pandas version is higher than ``0.14.0``. If it is below that you would have to update Pandas by running 
                        
                        ``pip install pandas --upgrade`` 
                        
                * You may have to upgrade ``numpy`` and ``numexpr`` before upgrading ``pandas`` if upgrading ``pandas`` gives you an error. 
                The command to upgrade numpy and numexpr is the same 
                
                        ``pip install numpy --upgrade`` 
                        
                        ``pip install numexpr --upgrade`` 
                        
        #. Download the code from MOOCdb github. The three code repositories that you would download are 
         *``https://github.com/MOOCdb/Translation_software/tree/master/edx_to_MOOCdb_piping/import.openedx.diagnosis``
         
         *``https://github.com/MOOCdb/Translation_software/tree/master/edx_to_MOOCdb_piping/import.openedx.apipe``
         
         *``https://github.com/MOOCdb/Translation_software/tree/master/edx_to_MOOCdb_piping/import.openedx.qpipe``
         
         
**************************************
Step 2: Running the translation software 
**************************************

If your course is through edX you would get the files shown below. The most important and perhaps most tedious is
processing the tracking log files. Some of the files listed below in the table could be representative of what MIT delivers to us. But tracking_log.json is the largest file
and contains the detailed clickstream events. These are the events which are recorded along with event type. 

.. list-table::
   :widths: 40 10 70
   :header-rows: 1

   * - File
     - Type
     - content
   * - <course name>__profiles.csv 
     - csv
     - contains PII information about the learner
   * - <course name>__tracking_log.json 
     - json
     - Clickstream events stored as JSON logs
   * - <course name>__studentmodule.csv 
     - csv
     - Student state information 
   * - <course name>_user_id_map.csv 
     - csv
     - mapping between username, id and hashid 
   * - <course name>__certificates.csv  
     - csv
     - information about certificates for each user_id
   * - <course name>_users.csv
     - csv
     - PII information + meta information like date_joined, last login etc
   * - <course name>__course_structure-prod-analytics.json 
     - JSON
     - Course structure in JSON
   * - <course name>_wiki_article.csv 
     - csv
     - contains the wiki article information
   * - <course name>__enrollment.csv  
     - csv
     - Contains information about enrollment 
   * - <course name>__wiki_articlerevision.csv 
     - csv
     - Contains information about wiki article revisions done by the students
   * - <course name>__forum.mongo
     - csv
     - contains forum posts etc made by the users 

  

One of the problem with our current delivery is that a user is identified by a number of items ; id, user_id, username, hashid, name, first_name, last_name 
and it is not clear how they are linked and where they are redundancies. We automatically link and clean this up and create a hash_id per 
user and have mechanisms to store user information with multiple hash. 

    #. Unzip tracking log file
        All raw data files in ``data/raw/<course_name>`` have the same prefix in the format of ``<course_name>__<creation date>``, we will 
        call the prefix ``COURSE_PREFIX``

        From within the tracking log file folder, run command:
   
          ``gzip -d COURSE_PREFIX__tracking_log.json.gz``
      
        This will extract the tracking log file into .json format, ready to be piped.

    #. If there are multiple log files, merge all the log files for a single course into one log file 
    
    #. Run JSON to relation code

        This tutorial covers the transfer of JSON tracking log file to CSV files. The code is written by Andreas Paepcke from Stanford.
        JSON tracking log file is stored with other raw data files. We will call the raw data files ``raw data`` and the output CSV ``intermediary CSV``.

        Let us suppose that we want to pipe the course named <course_name>,
        We assume raw data is stored in the folder :
   
            ``/.../<course_name>/log_data/``
     
        Create a folder called intermeidary_csv under the folder named <course_name>
   
            ``/.../<course_name>/intermediary_csv/``
     
        Create another folder called moocdb_csv under the folder named <course_name>
   
            ``/.../<course_name>/moocdb_csv/``

    #. Launch the piping

        From within the import.openedx.json_to_relation folder, run command:

        ``bash scripts/transformGivenLogfiles.sh 
        /.../<course_name>/intermediary_csv/`` 
        
        ``/../<course_name>/log_data/COURSE_PREFIX__tracking_log.json``

        As show in the command above, transfromGivenLogFiles.sh takes two arguments. First argument is the path to the destination folder, 
        and second argument is the tracking log json file to pipe. ``/.../`` represents the path to the directory where the <course_name> folder is located on your machine. 
        The command may run for a few hours and depends on the size of the 
        raw json tracking log file.The output csv files will be in ``/.../<course_name>/intermediary_csv``. 

    #. Run relation to MOOCdb 
        This tutorial covers the transfer of CSV files as output by Andreas Paepcke’s json_to_relation to MOOCdb CSV files.
        We will call the source CSV ``intermediary CSV`` and the output CSV ``MOOCdb CSV``.

        Let us suppose that we want to pipe to MOOCdb the course named <course_name>.
        We assume that the course’s log file has been processed by json_to_relation, 
        and that the output files are stored in the folder :

              ``/.../<course_name>/intermediary_csv/``

        We want the MOOCdb CSV to be written to folder 

              ``/.../<course_name>/moocdb_csv/``

            a. Edit ``import.openedx.qpipe/config.py``
                **The variables not mentionned in the tutorial must simply be left untouched.**
      
            b. QUOTECHAR : the quote character used in the intermediary CSV files. Most commonly a single quote : ‘
   
            c. TIMESTAMP_FORMAT : describes the timestamp pattern used in ``*_EdxTrackEventTable.csv`` intermediary CSV file. 
               See python doc to understand syntax.
   
            d. COURSE_NAME: the name of the folder containing the intermediary CSV files. Here, <course_name>.
   
            e. CSV_PREFIX : All the intermediary CSV file names in 
   
                ``/.../<course_name>/intermediary_csv/``
         
                share a common prefix that was generated when running JSON to relation. 
      
                This prefix is also the name of the only .sql file in the folder. 
      
            f. DOMAIN: the domain name of the course platform URL. Most commonly, https://www.edx.org or https://courses.edx.org. 
               (No slash at the end of the domain name) 
               To be sure, you can look at the URLs appearing *_EdxTrackEventTable.csv intermediary CSV file.

    #. Launch the piping
        When the variables mentioned above have been correctly edited in ``config.py``, the script is ready to launch. 
        From within the ``import.openedx.qpipe`` folder, run command :
   
            ``time python main.py``

    #. Delete log file
        When the piping is done, if everything went well, go to the output directory ``/.../<course_name>/moocdb_csv/`` and 
        delete the ``log.org`` file that takes a lot of space.

    #. Load course into MySQL
        Copy the file ``/.../<course_name>/moocdb_csv/6002x_2013_spring/moocdb.sql`` to ``/.../<course_name>/moocdb_csv/`` folder.
        Change directory to ``/.../<course_name>/moocdb_csv/``
        Replace ``6002x_spring_2013`` by <course_name> in ``moocdb.sql`` file.

        Run command :

             ``mysql -u root -p --local-infile=1 < moocdb.sql``

        This creates a database named <course_name> in MySQL, and loads the CSV data into it. 



Translation details 
+++++++++++++++++++++
Some examples contextualized presented via the two urls below show for an actual course show how the translation from raw JSON logs to MOOCdb takes place  
        http://alfa6.csail.mit.edu/moocdbdocs/interaction-scenario.html
        
        http://alfa6.csail.mit.edu/moocdbdocs/problem-check-example.html
        
More details can be found in Quentin Agrens thesis here
        


