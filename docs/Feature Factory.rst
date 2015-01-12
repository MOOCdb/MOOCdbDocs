
============================
Feature Factory Instructions 
============================

https://github.com/MOOCdb/MOOCdb/tree/master/feature_factory

https://github.com/MOOCdb/MOOCdb/tree/master/feature_discovery

Fresh setup:
#ultimate guide http://thecodeship.com/deployment/deploy-django-apache-virtualenv-and-mod_wsgi/


.. code:block:: python

   sudo apt-get update --fix-missing
   sudo apt-get install python-pip python-dev build-essential
   sudo pip install --upgrade pip
   sudo apt-get install apache2 apache2.2-common apache2-mpm-prefork apache2-utils libexpat1
   sudo apt-get install libapache2-mod-wsgi
   sudo service apache2 restart
   sudo pip install django celery django-celery django-kombu hoover django_extensions
   sudo apt-get install python-mysqldb 
 
.. code:block:: python

   sudo nano /etc/apache2/apache2.conf #add this
   ServerAdmin kiarashplusplus@gmail.com
   ServerName featurefactory.csail.mit.edu
   WSGIScriptAlias / var/www/featurefactory.csail.mit.edu/index.wsgi

.. code:block:: python

   sudo mkdir /var/www/featurefactory.csail.mit.edu
   sudo nano /var/www/featurefactory.csail.mit.edu/index.wsgi
   import os
   import sys

path = '/home/ubuntu/alfa'
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'alfa.settings'

import django.core.handlers.wsgi
application = django.core.handlers.wsgi.WSGIHandler()


copy files to ~/alfa


to send email:
sudo apt-get install postfix

sudo service apache2 restart
