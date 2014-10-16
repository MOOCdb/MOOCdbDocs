#############
Translation software documentation 
#############

This document describes in detail how the translation is done from the raw data files Coursera and edX provide and how to run the 
software and different assumptions and heuristics used during the translation.Currently translation software is available for 
three different platforms-edX, OpenedX and Coursera. 


edx
===

Data from edX platform is delivered as the following files 
**********************
Files from edX for a course called <course name>
**********************

.. list-table::
   :widths: 10 70
   :header-rows: 1

   * - Date
     - Change
   * - 10/16/14
     - Updated video events with new fields relating to mobile device use in
       the :ref:`Tracking Logs` chapter.
   * - 10/07/14
     - Added new student and instructor events relating to cohort use to the
       :ref:`Tracking Logs` chapter.
   * - 
     - Removed information about XML course formats. See the `edX Open
       Learning XML Guide <http://edx-open-learning-
       xml.readthedocs.org/en/latest/index.html>`_ for information about
       building XML courses.

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

