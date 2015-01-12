
=======
MOOCviz
=======



For info on MOOCviz, including purpose, features, and architecture, read the following papers:

MoocViz: A Large Scale, Open Access, Collaborative, Data Analytics Platform for MOOCs

MOOCdb: An collaborative environment for MOOC data

MOOCviz 2.0: A Collaborative MOOC Analytics Visualization Platform

Consult Kalyan for access to the last two.

If you have questions about anything and can't find the answer here on this wiki, feel free to email me at pwrtsoccer@gmail.com and I will do my best to help you.

-Setup

Follow the tutorial ( see below) to set up for developing and upgrading MOOCviz.

-Server

I tried my best to setup the server according to this guide (http://robmclarty.com/blog/how-to-setup-a-production-server-for-rails-4), however it is not all the same. The server uses Apache 2 to deploy the Rails 4 MOOCviz web application.

-Capistrano

Capistrano (https://github.com/capistrano/capistrano) is a remote server automation tool. At one point I tried integrating Capistrano into MOOCviz using this guide (http://robmclarty.com/blog/how-to-deploy-a-rails-4-app-with-git-and-capistrano), but there were more important things to be done and I had at least a somewhat consistent method for upgrading the server with new features. However, it would be much better to have Capistrano integrated as it offers lots of cool features and is less error prone than updating the server yourself. I pushed the work I had done to another branch on GitHub here.


—Tutorial for MOOCviz 

Clone from GitHub

Clone the project from GitHub onto your local machine. Once you've done this, start a new branch to make any changes, push the branch to GitHub when you are finished, and use GitHub to compare and merge the branch into master. This way you can see merged branches and easily compare any changes you've made in case you need to backtrack.

Add your keypair on Nimbus

OpenStack is a cloud computing platform that hosts virtual machines. Nimbus is CSAIL's OpenStack account. The virtual machine instance that hosts MOOCviz is named moocviz-prod. The IP address is 128.52.128.146, which is registered under MIT's WebDNS for the domain name moocviz.csail.mit.edu. For enough RAM to run a Rails-Apache server, the instance needs an s1.4 core size.

To be able to SSH into the instance, you'll need to have the moocviz keypair private file. Due to a misunderstanding when creating the snapshot, the associated keypair is prestonthompson, so you'll need the prestonthompson.pem file. Talk to Kalyan about getting this.

You won't need my password, just the keypair file, and if at some point a new instance is created, you can simply use another keypair and forget about mine.

Managing the server

Upgrading MOOCviz

When you've finished developing a new feature, pushed it to GitHub master branch, and need to upgrade MOOCviz production, fire up a terminal and follow these steps:

Backup the production database on your computer scp -i path/to/prestonthompson.pem ubuntu@128.52.128.146:/var/www/MOOCdb/moocenimages/db/production.sqlite3 path/to/store/db/backup

SSH into the server ssh -i path/to/prestonthompson.pem ubuntu@128.52.128.146

Change into the MOOCviz Rails directory ubuntu$ cd /var/www/MOOCdb/moocenimages

Backup the current branch git branch backupXX

Pull the changes from master on GitHub git pull

Migrate the database if there are any migrations bundle exec rake db:migrate RAILS_ENV=production

Precompile the changed assets bundle exec rake assets:precompile RAILS_ENV=production

Restart Apache sudo service apache2 restart

Restarting when OpenStack Fails

If for some reason Nimbus fails and needs to be restarted (e.g. power outage), follow these steps to get the server back up and running:

Log into NIMBUS take a snapshot of the current instance (named moocviz-prod)

Wait until the snapshot is completely finished, then delete that instance

Launch a new instance from that snapshot. The instance should be set to IP address 128.52.128.146 using an s1.4 core size.
