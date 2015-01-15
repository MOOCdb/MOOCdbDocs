
Coursera
=========


Setting Up the Coursera-MOOCdb Transformation Scripts
----------------------------

Pre-requisites
~~~~~~~~~~~~~~~
In order to set up MOOCDB and transform courses, the following are required: 

* Elementary Python programming
* Installing Python packages
* Elementary usage of any MySQL client
* Creating MySQL databases and importing data from SQL dumps into MySQL databases
* Familiarity with the types of databases Coursera exports and what kind of data is stored in each

Installing Dependencies
~~~~~~~~~~~~~~~~~

Before setting up MOOCdb, you will need to install Python, and a number of python modules listed below. 
Python
MOOCdb was tested and found to work on:

* Python 2.7

Python Modules
~~~~~~~~~~~

You will need to install the following Python modules:

* **MySQLdb** (mandatory): pip install MySQL-python
* **phpserialize** (req. for Coursera data piping): pip install phpserialize

Setting Up MOOCdb
~~~~~~~~~~~~~

Before converting a course to MOOCdb, there are two databases you need to create and import into. 
MOOOCdb Core
This database is required by all transformation tasks, but no task will ever alter its data. Hence, you can import it once and use it many times. Call this database **moocdb_core**, and avoid using this name for any other databases. After creating this empty database, import the file ``.../core/moocdb_core.sql`` located inside the piping scripts directory 
 Note that moocdb_core does not have to reside on the same machine on which the output database will be built. 
The schema
This database is also required for all transformations, and can be created once and used many times. Call this database **moocdb_clean**, and avoid using this name for any other databases. Unlike moocdb_core, this database has to be created on the same machine on which the output databases will be created. After you create **moocdb_clean** as an empty database, import the file .../core/schema.sql located inside the piping scripts directory. 
Using MOOCdb

You are now ready to transform a Coursera course to MOOCdb.


To do so, you have to supply a set of parameters telling the program where to find the input data, where to put the output data, and specifying other required information and options. To transform a course, you simply have a call a **main** function with a dictionary of these parameters. The scripts expect this dictionary to follow a very specific structure. To familiarize yourself with the parameter dictionary structure and how to call the **main** transformation function, please look at the **run_coursera.py** file located in the piping scripts directory. In this file, the main function is first imported from main.py. Second, the parameter dictionary is constructed. The variable holding it in the example is called **vars**. Finally, main(vars) is called. This should start the transformation. Below is an explanation of the parameters you need to specify. Before, we start, however, you will need to watch out for the following.


Coursera changed its export format at some point in the past. Previously, table columns referring to anonymized user IDs (such as in the lecture_submission_metadata) in the anonymized_general database were called **anon_user_id**. User IDs were anonymized in the anonymized_forum database as well and placed in columns called **forum_user_id**. If your course following this format, we will refer to its format as **coursera_1**.


More recently, Coursera changed the **anon_user_id** to **session_user_id**, and stopped anonymizing user IDs in the anonymized_forum database. If your course follows this format, we will refer to its format as **coursera_2**.


These naming changes will also be visible in the hash_mapping table directly. If your course does not strictly adhere to one of these 2 formats, this program might not be able to handle its data. Please request a new export of your course from Coursera. You should receive it in the second format regardless of when the course ran.


**source** (Coursera)

Contains the parameters related to the input (Coursera side)
• **platform_format**: **coursera_1** or **coursera_2**
• **course_id**: For **coursera_2** courses, set this to None. For **coursera_1** courses, lookup the numeric part of any of the kvs tables in the anonymized_general database, and write it here as a number. For example, if you see a table called kvs.136.quizzes, then set this parameter to 136
• **course_url_id**: This must be the session ID of the course, as a string (ex: **algorithms-001**)
•**host**, **user**, **password**, **port**: The MySQL connection parameters to the server hosting the coursera databases. If you do not know which port the MySQL server uses, try the default value (3306).
•**hash_mapping_db**: The name of the course hash-mapping database
•**general_db**: The name of the course anonymized-general database
•**forum_db**: The name of the course anonymized-forum database

**core**

Contains the parameters required to connect to the MOOCdb-core database.
•**host**, **user**, **password**, **port**: The MySQL connection parameters to the server hosting the moocdb_core database. If you do not know which port the MySQL server uses, try the default value (3306)

**target** (Independent of source platform)

Contains the MOOCdb output and clean database connection parameters.
•**host**, **user**, **password**, **port**: The MySQL connection parameters to the server hosting the MOOCdb output and clean databases. If you do not know which port the MySQL server uses, try the default value (3306)
•**db**: The name of the MOOCdb output database

**options**
Sets transformation options.
•**log_path**: The path in which the log file for the transformation should be placed. This should be a path to a directory not a file. The log file for a single transformation task will be placed inside that directory and will be named based on the course_url_id and the task start date/time.
•**log_to_console**: True | False, whether or not log messages should also be written to the console.
•**debug**: True | False. If True, the script will only transform data for a limited number of users, speeding up the run for development purposes.
•**num_users_debug_mode**: The number of users to transform in debug mode

Checking the Outputs:
~~~~~~~~~~~~~~~~~~

After a transformation process has exited, please check the console and log file to verify that it completed successfully. Also, please visit the MySQL server hosting the output database and make sure its tables are populated. 

