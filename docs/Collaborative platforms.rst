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
Our current version of this platform can be seen at .._Feature Factory : http://featurefactory.csail.mit.edu

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

We developed an online platform where users would post their tagging projects and a crowd of helpers can p
articipate in MOOC data science by selecting a project and tagging the content based on some instructions. 
We call the online collaborative platform that serves this purpose Label Me-Text. 
Label Me’s current incarnation is shown in Figure 3.  It works in the following ways: 

-	Users requiring annotation of natural language can create an annotation project by providing a csv file for the content, instructions and examples for tagging. 

-	Taggers (Label Me’s crowd) can participate by selecting a project, following the instructions and tagging the content 

-	A database consisting of tags for the content for the project is initialized and populated as taggers work. A number of analytic services are provided around this database such as – evaluation of inter rater reliability, summary of tags, summary of activity for a project (how many taggers helped, time series of number of tags).

-	A service can be called upon to filter the tagged data based on the reliability measures just mentioned. It then uses methods based upon latent semantic analysis to learn a tagging model. 

-	Taggers (Label Me’s crowd) are given credit for every tag they have provided and the number of their tags that pass the filters to be used in model learning. 


System Requirements 
-------------------




Installation steps 
-------------------


Testing 
-------------------


Curate Me  
==========

Coming soon
