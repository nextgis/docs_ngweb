.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>
.. sectionauthor:: Dmitry Baryshnikov <dmitry.baryshnikov@nextgis.com>

.. _ngw_install_ubuntu:    

Install in Ubuntu
==================

This document is checked and subject to work with Ubuntu Server 14.04 LTS or higher.
Python 2.7 is required to perform an installation. 

.. warning:: 
   To support file names with local names (e.g. ru_RU.UTF-8) 
   it is required that appropriate locale is installed.
   To check if locale is installed use a command: locale -a
   To add a new locale use a command: locale-gen ru_RU.utf8

Preparation of the database
--------------------------------

Add the ubuntugis repository (see  `supported distributions <http://trac.osgeo.org/ubuntugis/wiki/SupportedDistributions>`_):

.. code:: bash

   $ sudo apt-get update
   $ sudo apt-get install software-properties-common python-software-properties

**For ubuntu server 14.04**

.. code:: bash
    
    $ sudo apt-add-repository ppa:ubuntugis/ubuntugis-unstable
    $ sudo apt-get update
    $ sudo apt-get upgrade

Install PostgreSQL:

.. code:: bash

    $ sudo apt-get install postgresql-<version>

Create a user who would be used as database.user in
config.ini (see  further):

.. code:: bash

    $ sudo -u postgres createuser ngw_admin -P -e

enter a password three times and then input  'n'.

Create a database where NGW will be deployed, the name of database should be the 
same as database.name in config.ini (see further):

.. code:: bash

    $ sudo -u postgres createdb -O ngw_admin --encoding=UTF8 db_ngw
    $ sudo nano /etc/postgresql/9.3/main/pg_hba.conf

Edit a file so it contains the following strings (change authentication method 
to ``md5``, if another is specified):

.. code:: bash

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5
    # IPv6 local connections:
    host    all             all             ::1/128                 md5


Do not forget to restart database service:

.. code:: bash

    $ sudo service postgresql restart

Install PostGIS:

.. code:: bash

    $ sudo apt-cache search postgis

Find a package in the list that suits for your PostgreSQL version, its name 
should look like postgresql-{version}-postgis-{version}, and install it:

.. code:: bash

    $ sudo apt-get install postgresql-9.3-postgis-2.1
    $ sudo -u postgres psql -d db_ngw -c 'CREATE EXTENSION postgis;'
    $ sudo -u postgres psql -d db_ngw -c 'ALTER TABLE geometry_columns OWNER \ 
    TO ngw_admin;'
    $ sudo -u postgres psql -d db_ngw -c 'ALTER TABLE spatial_ref_sys OWNER \
    TO ngw_admin;'
    $ sudo -u postgres psql -d db_ngw -c 'ALTER TABLE geography_columns OWNER \
    TO ngw_admin;'

After these operations databases will be created in PostgreSQL with installed 
:term:`PostGIS` and a user :abbr:`DB (database)`, will become an owner of 
databases, and also an owner of ``geometry_columns``, ``georgaphy_columns``, 
``spatial_ref_sys`` tables.

Check if PostGIS functions appeared in a database:

.. code:: bash

    $ psql -h localhost -d db_ngw -U ngw_admin -c "SELECT PostGIS_Full_Version();"

If you deploy a system on a clean server and need to create one more PostGIS 
database to store data, you may enable a network access to it

.. code:: bash

    $ sudo su - postgres
    $ nano /etc/postgresql/9.3/main/pg_hba.conf
    add a sting to the end of file: host  all    all    192.168.0.0/16    md5
    mask 192.168.0.0/16 allows to access to DB from all IP starting with "192.168"

    $ nano /etc/postgresql/9.3/main/postgresql.conf
    create a string listen_addresses='*' and uncomment it.

.. code:: bash

    $ sudo service postgresql restart

Preparation of basic software
------------------------------

Install pip:

.. code:: bash

    $ sudo apt-get install python-pip

Install virtualenv:

.. code:: bash

    $ sudo pip install virtualenv

Install additional tools:

.. code:: bash

    $ sudo apt-get install python-dev git libgdal-dev python-dev \
    g++ libxml2-dev libxslt1-dev gdal-bin libgeos-dev zlib1g-dev libjpeg-turbo8-dev

After an update of NextGIS Web a key registration may be required. 
**For most cases there is no need to generate keys.**  This is required for development.

Generate keys to work with GitHub (copy and paste a key to user settings at GitHub in `SSH keys section <https://github.com/settings/ssh>`_):

.. code:: bash

    mkdir ~/.ssh
    cd ~/.ssh
    ssh-keygen -t rsa -C "your@email.com"
    ssh-add ~/.ssh/id_rsa
    cat id_rsa.pub
    cd ~

If you enebled two-factor authentication you need to:

* `Cache a password <https://help.github.com/articles/caching-your-github-password-in-git/#platform-linux>`_
* `Generate an access token <https://github.com/settings/applications#personal-access-tokens>`_
  and use it instead of password


.. _ngw_install_prepare:

Prepare to NextGIS Web installation
--------------------------------------

Create required directories:

.. code:: bash

    $ mkdir -p ~/ngw/{data,upload}
    $ cd ~/ngw

Clone repository:

.. code:: bash

    $ git clone https://github.com/nextgis/nextgisweb.git

Create a virtual environment in a folder ``~/ngw/env`` (folder will be created after a command execution):

.. code:: bash

    $ virtualenv --no-site-packages env

.. _ngw_install:

NextGIS Web installation
-------------------------

Install NextGIS Web package in development mode. All required packages will be installed:

.. code:: bash

    $ env/bin/pip install -e ./nextgisweb

Install MapServer
-------------------

This module needs a MapScript to function. Mapscript is not installed to a virtual environment in a standard way so you need to install it manually.

Install required package to a system:

.. code:: bash

    $ sudo apt-get install python-mapscript

After that you need to copy required files to a virtual environment directory used for NextGIS Web. At this steps there are at least two options depending on what kind of python-mapscript package is installed to a system. This depends on distributive used.

If you use Ubuntu, to copy system MapScript to virtual environment (directory ``env``) you can use the following commands:

.. code:: bash

    $ mkdir env/lib/python2.7/site-packages/mapscript.egg
    $ cp /usr/lib/python2.7/dist-packages/*mapscript* \ 
    $ env/lib/python2.7/site-packages/mapscript.egg
    $ echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

If you use FreeBSD, the process will be slightly different:
    
.. code:: bash

    $ cp -r `python -c "import mapscript, os.path; print \ 
    os.path.split(mapscript.__file__)[0]"` env/lib/python2.7/site-packages/mapscript.egg
    $ echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

If you use Fedora/CentOS, then:

.. code:: bash

    $ mkdir env/lib/python2.7/site-packages/mapscript.egg
    $ cp /usr/lib/python2.7/site-packages/*mapscript* \ 
    env/lib/python2.7/site-packages/mapscript.egg
    $ echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

If you execute a command:

.. code:: bash

    $ env/bin/pip freeze

you will get an error message:

::

    "Missing 'Version:' header and/or PKG-INFO file", mapscript [unknown version]

To fix it you create a file ``PKG-INFO``:

.. code:: bash

    $ mkdir env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO
    $ touch env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO

Set the version of MapScript to be used:

.. code:: bash

    $ echo `python -c "import mapscript; print 'Version: %s' % mapscript.MS_VERSION"` \
    > env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO


.. _ngw_mapserver_install:

Install NextGIS Web MapServer
-------------------------------

Clone repository: 

.. code:: bash

    $ git clone https://github.com/nextgis/nextgisweb_mapserver.git

Install a package in development mode:

.. code:: bash

    $ env/bin/pip install -e ./nextgisweb_mapserver

Execute a command one more time:

.. code:: bash

    $ env/bin/pip freeze

to check if there any errors.

Also you can install QGIS rendering module, and get rendered maps looking same as in desktop NextGIS QGIS. See install manual at ":ref:`ngw_install_qgis`".

NextGIS Web configuration file
---------------------------------

Configuration file with default parameters could be created using a command ``nextgisweb-config``:

.. code:: bash

    $ env/bin/nextgisweb-config > config.ini

A configuration file ``config.ini`` will be created. This text file should be edited to match the environment. Purpose of parameters is described in comments. User name and password and also a directory for data storage are taken from commands above. Check if the following parameters a set correctly:


Example of NextGIS Web configuration file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: 

	[file_upload]
	
	# Temporary directory for storage of uploaded files
	# path =

	[pyramid]

	# Key used to encrypt cookies (required) 
	secret =  
	# HTML-help 
	help_page = /home/trolleway/ngw/help.htm
	# System logo 
	# logo = 
	# Favicon 
	# favicon = 
	# Redirect link when open / 
	# home_url = 

	[core]

	# System name 
	system.name = NextGIS Web
	# Full system name 
	system.full_name = Demo web gis
	# Database host name 
	database.host = localhost
	# Database name 
	database.name = db_ngw
	# Database user name 
	database.user = ngw_admin
	# Database user password 
	database.password =  
	# Check the connection at startup 
	# database.check_at_startup = 
	# Do not load listed packages 
	# packages.ignore = 
	# Do not load listed components 
	# components.ignore = 
	# Data storage directory 
	sdir = /home/trolleway/ngw/data
	# Locale used by default
	locale.default = ru

	[file_storage]

	# Directory for storage of files 
	# path =

	[feature_layer]

	# Show attributes in identify 
	# identify.attributes = 

	[webmap]

	# File with base layers definitions 
	# basemaps = 
	# Bing Maps API-key 
	# bing_apikey = 
	# Identification tolerance 
	# identify_radius = 
	# Width of pop-up window 
	# popup_width = 
	# Height of pop-up window 
	# popup_height = 

	[wmsclient]


	[mapserver]

	# List of fonts in a MAPFILE FONTSET format 
	# fontset = 



To generate a key for configuration file ``config.ini`` use a command

.. code:: bash
	
	$ openssl rand -base64 16

.. warning::
   1. In some cases absolute paths to folders should be entered because a python parameter 
   %(here)s works not in all cases.
   2. No spaces are allowed before the name of variable in configuration file
   .

Also commands pserve or pshell require a paster configuration file, e.g. ``development.ini``.

.. code:: bash

    $ nano development.ini

Contents:

::

    [app:main]
    use = egg:nextgisweb

    # a path to the main configuration file
    config = %(here)s/config.ini

    # a path to logging library configuration file
    # logging = %(here)s/logging.ini

    # parameters useful for debugging
    # pyramid.reload_templates = true
    # pyramid.includes = pyramid_debugtoolbar

    [server:main]
    use = egg:waitress#main
    host = 0.0.0.0
    port = 6543

If it is supposed that a server will be used in Intranet only then you need to delete records about Google basemaps from 
/nextgisweb/nextgisweb/webmap/basemaps.json.

Internationalization and Localization
---------------------------------------

As compiled files with translated interface are not stored inside a version control system you need to compile them for each package, or the administrator interface will be in English:

.. code:: bash

    $ env/bin/nextgisweb-i18n --package nextgisweb compile
    $ env/bin/nextgisweb-i18n --package nextgisweb_mapserver compile
    
To install localization by default for Russian language you need to add a string to a section **core** of configuration file (e.g. **config.ini**):

.. code:: ini

   locale.default = ru
   
So the interface will be Russian during the first launch.   

Database initialization
------------------------

If you plan to use an interface language other than English by default make sure you have compiled translation files and that a ``locale.default`` setting of a ``core`` component in configuration file``config.ini`` is set to required language before initialization of database, in other case some strings will remain English even after a forced change of language in administrator interface.

To initialize a database follow these steps:

.. code:: bash

    $ env/bin/nextgisweb --config config.ini initialize_db

In some cases, e.g. during update, you may need to remove all database data and initialize a database one more time:

.. code:: bash

    $ env/bin/nextgisweb --config config.ini initialize_db --drop


Migration and backup
--------------------------------

Migration - is a process of data and NextGIS Web transfer between servers. During a migration a backup is created for:

* All the content of NextGIS Web database: information about layers, styles, user 
  accounts, so everything that is set in administrator interface.
* Vector data uploaded through administrator interface.
* Raster data uploaded through administrator interface. 

Config.ini file is not included to backup, it should be transfered separately.

To start a process of migration execute the following commands:

.. code:: bash

	$ env/bin/nextgisweb --config config.ini backup file.ngwbackup
	$ env/bin/nextgisweb --config config.ini restore file.ngwbackup

Backup is a ZIP-archive. To disable archiving of backup you need to use a key —no-zip. A catalog with defined name would be created.

.. code:: bash

	$ env/bin/nextgisweb  --config "config.ini" backup "backup/ngwbackup" --no-zip

In FreeBSD OS there is an error: sqlite support is not transfered in virtualenv. You need to manually copy the file:

.. code:: bash

	$ cp /usr/local/lib/python2.7/site-packages/_sqlite3.so \
	env/lib/python2.7/site-packages/


Migration should be performed using these steps:

1. Start a backup on source server.

.. code:: bash

	$ env/bin/nextgisweb  --config "config.ini" backup "backup/ngwbackup" --no-zip

2. If you need to transfer a PostGIS database with geodata then you need 
   to make its backup using a pgAdminIII software in a tar format.
3. On target server install NextGIS Web using a manual (see  section 2).
4. NextGIS Web database is created on target server and  
   access rights are set using pgAdminIII.
5. On a target server in the config.ini file you need to set a connection to a database for 
   NextGIS Web.

 
.. code::

	# Database host name 
	database.host = localhost
	# Database name 
	database.name = zapoved_ngw
	# Database user name 
	database.user = user
	# Database user password 
	database.password = password


6. On a target server execute a command: 

.. code:: bash

	$ env/bin/nextgisweb  --config "config.ini" restore "backup/ngwbackup"

7. Launch NextGIS Web. Everything should work except PostGIS layers (if there were any).  
   
8. If you need to transfer a PostGIS database with geodata you need to create a new database 
   and then deploy a backup from source server.
9. You need to enter a new server address in PostGIS connection settings. 

If there is an error "No module named pysqlite2" - it means that you forgot to 
transfer sqlite. Execute required command from the installation manual.


Software update
-----------------

To update NextGIS Web software execute a command:

.. code:: bash

	$ cd ~/ngw/nextgisweb
	$ git pull
	
If some dependences were added to setup.py you need to execute:	

.. code:: bash

	$ env/bin/pip install -e ~/ngw/nextgisweb
	
If a database structure has changed you need to execute:	

.. code:: bash

	$ cd ../
	$ env/bin/nextgisweb --config config.ini initialize_db

Also you need to update nextgisweb_mapserver package:

.. code:: bash

	$ cd ./nextgisweb_mapserver
	$ git pull

After executing of commands you need to restart NextGIS Web software with a restart of 
pserve, or with a restart of web server with uWSGI module.


Warnings and errors
-----------------------

During a work of software some diagnostic messages may be displayed in pserver console or written to the log:

.. code:: bash

    ault.py:471: SAWarning: Unicode type received non-unicode bind param value.
    processors[key](compiled_params[key])

This message is not important.

If you plan to work with API from leaflet or OpenLayers you need to setup CORS technology.s
