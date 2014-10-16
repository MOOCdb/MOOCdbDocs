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

.. list-table::
   :widths: 10 70 40
   :header-rows: 1

   * - File
     - Type
     - content
   * - sql 
     - contains student state
     - test
  

primarily stored in JSON logs. 


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

