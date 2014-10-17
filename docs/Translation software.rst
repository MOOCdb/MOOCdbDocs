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

