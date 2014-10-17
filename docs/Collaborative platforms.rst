############
Collaborative platforms
############

This documentation is for folks to install different open source platforms that MOOCdb project has. They include 
Collaborative visualization platform - MOOCviz, platform to crowd source feature and variable formation - Feature Factory,
platform for getting labels from the crowd - LabelMe-Text. These instructions allow folks to create their own instantiation of the 
platform for internal use. The instructions will enable to download the software and deploy a server for each one of these platforms. 


MOOCviz
=======


A MOOCviz platform offers:

-	A central, shared gallery of visualizations with a demonstration list of courses for which the participant-generated visualizations have been rendered. 

-	The ability for the participants to download the software that generates visualizations and execute it over their own course data that is formatted in MOOCdb schema. They will also be able to automatically package the resulting rendered visualization and upload it to the gallery, adding it to the demonstration list. 

-	A means to contribute software for new visualizations to the gallery via the MOOCviz web-based interface. 

-	A means of commenting on any existing visualization by posting in the comments section underneath it. Discussions are free form. They likely will extend beyond the interpretation or thoughts provoked by the visualization to the ways that the data have been transformed in extraction and aggregate steps. We expect that discussions will stimulate ideas for new visualizations.

System Requirements 
-------------------




Installation steps 
-------------------


Testing 
-------------------


Feature Factory
===============

We are developing a second web-based collaborative platform called Feature Factory. 
Our current version of this platform can be seen at .._Feature Factory:http://featurefactory.csail.mit.edu. 

Feature Factory offers two modes of engagement:

- The “solicit” mode is used by MOOC data science, education technology, or learning science research teams. A team describes the outcome it is currently studying or trying to predict. It explicitly explains what features or explanations are sought and it solicits help from the “MOOC crowd”. 

- In the second mode, “helping”, the MOOC crowd proposes, explanations or variables, and suggests means to operationalize them. They provide comments on proposal or vote them up or down in popularity. The software savvy among them write and share software scripts written to operationalize the most popular or compelling proposals. 


System Requirements 
-------------------




Installation steps 
-------------------


Testing 
-------------------

LabelMe- Text 
=============

Data from edX platform is primarily stored in JSON logs. A fundamental axis which is used to record precisely the activity performed 
by the learner is an "event type". Multiple "event types" differentiate between different activities done by the learner. We base
our software on this fundamental axis. Below we provide detailed description of how each event type is translated into an entry in 
MOOCdb. This detailed information gives researchers and plaform providers information about MOOCdb translation and how data is mapped 
syntactically and semantically. 

System Requirements 
-------------------




Installation steps 
-------------------


Testing 
-------------------


Curate Me  
==========

Coming soon
