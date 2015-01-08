#############
Translation software documentation 
#############

This document describes in detail how the translation is done from the raw data files Coursera and edX provide and how to run the 
software and different assumptions and heuristics used during the translation. Currently translation software is available for 
three different platforms - edX, OpenedX and Coursera. 

===
edx
===

To process the tracking log files and get into MOOCdb format, we provide the following detailed instructions. First step is to install a machine with 
required packages and disk space. 

**************************************
Step 1: Installing the required packages or downloading a VM 
**************************************

* Option 1: To run the software you may want to download a VM from Amazon cloud. 
 
      This virtual machine image comes with all packages installed which are required to run the MOOCdb pipeline. To get a link to the image and request the '.pem' file, please email
      kalyan@csail.mit.edu. 
      
      
 .. important:: 
  
   * When instantiating this virtual machine on Amazon or locally, please provision the disk space (hard disk) 
     such that there is atlease three times the size of the decrypted- uncompressed file size of the tracking logs.
  
  
 * Option 2: Install all the packages on your local machine 
 
        The following packages are required on a MOOCdb machine 
        
        #. Install **Unidecode** package `available here`_
        
        #. Install **ijson** package which can be `found here`_
        
        #. Install **python-setuptools**
        
        #. Install **pip** using **sudo easy_install pip**
        
        #. Install **pandas**
        
        #. Download the code from MOOCdb github:
        
           * `Openedx diagnosis`_
         
           * `Openedx apipe`_
         
           * `Openedx qpipe`_
         
         .. _available here: https://pypi.python.org/pypi/Unidecode
         .. _found here: https://pypi.python.org/pypi/ijson
         .. _openedx diagnosis: https://github.com/MOOCdb/Translation_software/tree/master/edx_to_MOOCdb_piping/import.openedx.diagnosis
         .. _openedx apipe: https://github.com/MOOCdb/Translation_software/tree/master/edx_to_MOOCdb_piping/import.openedx.apipe
         .. _openedx qpipe: https://github.com/MOOCdb/Translation_software/tree/master/edx_to_MOOCdb_piping/import.openedx.qpipe
         
 .. Note::   
 
  * Make sure your Pandas version is higher than **0.14.0**. If it is below that you would have to update Pandas by running: 
  
                        **pip install pandas --upgrade**
                        
  * You may have to upgrade **numpy** and **numexpr** before upgrading **pandas**, if upgrading **pandas** gives you an error. The command to upgrade numpy and numexpr is the same:

                       **pip install numpy --upgrade** 

                       **pip install numexpr --upgrade**
**************************************
Step 2: Processing the tracking logs  
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


.. note:: Given the table of the data and types we now go through the steps you have to take to transform the log files. 

   #. Unzip tracking log file:
    
      All raw data files in **'data/raw/<course_name>'** have the same prefix in the format of '**<course_name>__<creation date>**', we will call the prefix '**COURSE_PREFIX**'.

      From within the tracking log file folder, run the command:
        
        ``gzip -d COURSE_PREFIX__tracking_log.json.gz``
 
      This will extract the tracking log file into .json format, ready to be piped.

   #. If there are multiple log files, merge all the log files for a single course into one log file.
    
      
   #. Run JSON to relation code (a.k.a apipe):

        This tutorial covers the transfer of JSON tracking log file to CSV files. The code is written by Andreas Paepcke from Stanford.
        JSON tracking log file is stored with other raw data files. We will call the raw data files as '**raw data**' and the output CSV as '**intermediary CSV**'.

        Let us suppose that we want to pipe the course named '**<course_name>**'. We assume that the raw data is stored in the folder:
   
            ``/.../<course_name>/log_data/``
     
        Create a folder called intermeidary_csv under the folder named '**<course_name>**'
   
            ``/.../<course_name>/intermediary_csv/``
     
        Create another folder called moocdb_csv under the folder named '**<course_name>**'
   
            ``/.../<course_name>/moocdb_csv/``

   #. Launch the piping:

        From within the import.openedx.json_to_relation folder, run command:

        ``bash scripts/transformGivenLogfiles.sh 
        /.../<course_name>/intermediary_csv/``
        
        ``/../<course_name>/log_data/COURSE_PREFIX__tracking_log.json``

        As show in the command above, transfromGivenLogFiles.sh takes two arguments. First argument is the path to the destination folder, 
        and second argument is the tracking log json file to pipe. '**/.../**' represents the path to the directory where the <course_name> folder is located on your machine. 
        The command may run for a few hours and depends on the size of the 
        raw json tracking log file.The output csv files will be in '**/.../<course_name>/intermediary_csv**'. The following gives 
        an example of the output csv files produced for link5_10x course:
        
        
                        ``link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_ABExperimentTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_AccountTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_AnswerTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_CorrectMapTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_EdxTrackEventTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_EventIpTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_InputStateTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_LoadInfoTable.csv
                        link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql_StateTable.csv``
        

   #. Run relation to MOOCdb (a.k.a qpipe):
   
        This tutorial covers the transfer of CSV files as output by Andreas Paepcke’s json_to_relation to MOOCdb CSV files.
        We will call the source CSV as '**intermediary CSV**' and the output CSV as '**MOOCdb CSV**'.

        Let us suppose that we want to pipe to MOOCdb the course named **'<course_name>'**.
        We assume that the course’s log file has been processed by json_to_relation, 
        and that the output files are stored in the folder :

              **/.../<course_name>/intermediary_csv/**

        We want the MOOCdb CSV to be written to folder 

              **/.../<course_name>/moocdb_csv/**

            a. Edit **import.openedx.qpipe/config.py**
            
                **The variables not mentionned in the tutorial must simply be left untouched.**
      
            b. **QUOTECHAR**: The quote character used in the intermediary CSV files. Most commonly a single quote : ‘
   
            c. **TIMESTAMP_FORMAT**: describes the timestamp pattern used in '***_EdxTrackEventTable.csv**' intermediary CSV file. See python doc to understand syntax.
   
            d. **COURSE_NAME**: The name of the folder containing the intermediary CSV files. Here it is **'<course_name>'**.
   
            e. **CSV_PREFIX**: All the intermediary CSV file names in 
   
                        **/.../<course_name>/intermediary_csv/**
         
                share a common prefix that was generated when running JSON to relation. This prefix is also the name of the only .sql file in the folder. For example, in the above case this prefix would be :
                
                        ``link5_10x_trace_merged.2014-11-02T23_46_45.622627_28028.sql``
      
            f. **DOMAIN**: the domain name of the course platform URL, most commonly they are https://www.edx.org or https://courses.edx.org. 
               (No slash at the end of the domain name). To be sure, you can look at the URL's appearing '***_EdxTrackEventTable.csv**' intermediary CSV file.

   #. Launch the piping:
   
        When the variables mentioned above have been correctly edited in ``config.py``, the script is ready to launch. 
        From within the ``import.openedx.qpipe`` folder, run the command:
   
            ``time python main.py``

   #. Delete log file:
   
        When the piping is done, if everything went well, go to the output directory '**/.../<course_name>/moocdb_csv/**' and 
        delete the '**log.org**' file that takes a lot of space.

   #. Load course into MySQL:
   
        Copy the file '**/.../<course_name>/moocdb_csv/6002x_2013_spring/moocdb.sql**' to '**/.../<course_name>/moocdb_csv/**' folder.
        Change directory to '**/.../<course_name>/moocdb_csv/**'. Replace '6002x_spring_2013' by '<course_name>' in ``moocdb.sql`` file.

        Run the command:

             ``mysql -u root -p --local-infile=1 < moocdb.sql``

        This creates a database named '**<course_name>**' in MySQL, and loads the CSV data into it. 


Translation details 
+++++++++++++++++++++

Some examples contextualized presented via the two urls below show for an actual course show how the translation from raw JSON logs to MOOCdb takes place  

        * `Interaction Scenario`_
        
        * `Problem Check Example`_
        
        .. _Interaction Scenario: http://alfa6.csail.mit.edu/moocdbdocs/interaction-scenario.html
        .. _Problem Check Example: http://alfa6.csail.mit.edu/moocdbdocs/problem-check-example.html
        
More details can be found in Quentin Agrens thesis here
