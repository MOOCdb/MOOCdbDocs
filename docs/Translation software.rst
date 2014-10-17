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
Some of the files listed below in the table could be representative of what MIT delivers to us. But tracking_log.json is the largest file
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



#. Prerequisites

This tutorial covers the transfer of CSV files as output by Andreas Paepcke’s json_to_relation to MOOCdb CSV files.
We will call the source CSV “intermediary CSV” and the output CSV “MOOCdb CSV”.

Let us suppose that we want to pipe to MOOCdb the course named <course_name>.
We assume that the course’s log file has been processed by json_to_relation, 
and that the output files are stored in the folder :

  /data/csv/intermediary_csv/<course_name>

We want the MOOCdb CSV to be written to folder 

/data/csv/moocdb_csv/<course_name>

#. Create folder /data/csv/moocdb_csv/<course_name>

For minimal hassle, the MOOCdb CSV folder must have the same name as the intermediary CSV folder. Here, <course_name>. 

Edit import.openedx.relation_to_moocdb/config.py

The variables not mentionned in the tutorial must simply be left untouched.

QUOTECHAR : the quote character used in the intermediary CSV files. Most commonly a single quote : ‘

TIMESTAMP_FORMAT : describes the timestamp pattern used in *_EdxTrackEventTable.csv intermediary CSV file. See python doc to understant syntax.

COURSE_NAME: the name of the folder containing the intermediary CSV files. Here, <course_name>.

CSV_PREFIX : All the intermediary CSV file names in 
/data/csv/intermediary_csv/<course_name>
share a common prefix that was generated when running JSON to relation. This prefix is also the name of the only .sql file in the folder. 

DOMAIN: the domain name of the course platform URL. Most commonly, https://www.edx.org or https://courses.edx.org. (No slash at the end of the domain name) To be sure, you can look at the URLs appearing *_EdxTrackEventTable.csv intermediary CSV file.

#. Launch the piping
When the variables mentioned above have been correctly edited in config.py, the script is ready to launch. 

From within the import.openedx.relation_to_moocdb folder, run command :

time python main.py

#. Delete log file

When the piping is done, if everything went well, go to the output directory /data/csv/moocdb_csv/<course_name> and delete the log.org file that takes a lot of space.

#Load course into MySQL

Copy the file /data/csv/moocdb_csv/6002x_2013_spring/moocdb.sql to /data/csv/moocdb_csv/<course_name> folder.

Change directory to /data/csv/moocdb_csv/<course_name>

Replace ‘6002x_spring_2013’ by <course_name> in moocdb.sql file.

Run command :

mysql -u root -p --local-infile=1 < moocdb.sql

This creates a database named <course_name> in MySQL, and loads the CSV data into it. 



Translation semantics
+++++++++++++++++++++

A fundamental axis which is used to record precisely the activity performed 
by the learner is an "event type". Multiple "event types" differentiate between different activities done by the learner. We base
our software on this fundamental axis. Below we provide detailed description of how each event type is translated into an entry in 
MOOCdb. This detailed information gives researchers and plaform providers information about MOOCdb translation and how data is mapped 
syntactically and semantically. 

Tracklog Event types
-------------------

play_video
^^^^^^^^^^

problem_check
^^^^^^^^^^^^^

