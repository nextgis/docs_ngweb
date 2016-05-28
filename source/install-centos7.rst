.. sectionauthor:: Denis Rykov <denis.rykov@nextgis.com>

.. _ngw_install_centos7:

Install in CentOS 7
====================

This document is checked and subject to work in CentOS 7.
Python 2.7 is required to perform an installation.

.. warning:: 
   To support file names with local names (e.g. ru_RU.UTF-8) 
   it is required that appropriate locale is installed.
   To check if locale is installed use a command: locale -a
   To add a new locale use a command: locale-gen ru_RU.utf8

Preparation of the database
----------------------------

PostGIS is absent in the official repository of CentOS 7 so you need to add third 
party repository. To do this go to `address <http://yum.postgresql.org/repopackages.php>`_,
select the required package for current release of PostgreSQL and install it. 
For PostgreSQL 9.5 it will look like:

.. code:: bash

    $ sudo yum install https://download.postgresql.org/pub/repos/yum/9.5/redhat/rhel-7-x86_64/pgdg-centos95-9.5-2.noarch.rpm

To view a list of all packages available from this repository execute a command:

.. code:: bash

    $ yum list | grep pgdg95

Install PostgreSQL:

.. code:: bash

    $ sudo yum install postgresql95 postgresql95-server postgresql95-libs \
    postgresql95-contrib postgresql95-devel

Initialize a database and enable autolaunch with the system start:

.. code:: bash

    $ sudo /usr/pgsql-9.5/bin/postgresql95-setup initdb
    $ sudo systemctl start postgresql-9.5.service
    $ sudo systemctl enable postgresql-9.5.service

Create a user who would be used as database.user in
config.ini (see  further):

.. code:: bash

    $ sudo -u postgres createuser ngw_admin -P -e

Create a database where NGW will be deployed, the name of database should be the 
same as database.name in config.ini (see further):

.. code:: bash

    $ sudo -u postgres createdb -O ngw_admin --encoding=UTF8 db_ngw

Edit authentication parameters in corresponding file:

.. code:: bash

    $ sudo nano /var/lib/pgsql/9.5/data/pg_hba.conf

Edit a file so it contains the following strings (change authentication method 
to ``md5``, if another is specified):

.. code:: bash

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5
    # IPv6 local connections:
    host    all             all             ::1/128                 md5

Do not forget to restart PostgreSQL:

.. code:: bash

    $ sudo systemctl restart postgresql-9.5.service

Add a repository with PostGIS dependences:

.. code:: bash

    $ sudo yum install epel-release

Install PostGIS:

.. code:: bash

    $ sudo yum install postgis2_95
    $ sudo -u postgres psql -d db_ngw -c 'CREATE EXTENSION postgis;'
    $ sudo -u postgres psql -d db_ngw -c \ 
    'ALTER TABLE geometry_columns OWNER TO ngw_admin;'
    $ sudo -u postgres psql -d db_ngw -c \
    'ALTER TABLE spatial_ref_sys OWNER TO ngw_admin;'
    $ sudo -u postgres psql -d db_ngw -c \
    'ALTER TABLE geography_columns OWNER TO ngw_admin;'

After these operations databases will be created in PostgreSQL with installed 
:term:`PostGIS` and a user :abbr:`DB (database)`, will become an owner of 
databases, and also an owner of ``geometry_columns``, ``georgaphy_columns``, 
``spatial_ref_sys`` tables.

Check if PostGIS functions appeared in a database:

.. code:: bash

    $ psql -h localhost -d db_ngw -U ngw_admin -c "SELECT PostGIS_Full_Version();"

Preparation of basic software
--------------------------------

Install pip:

.. code:: bash

    $ sudo yum install python-pip

Install virtualenv:

.. code:: bash

    $ sudo yum install python-virtualenv

Install additional tools:

.. code:: bash

    $ sudo yum install git gdal gdal-devel libxml2-devel libxslt-devel gcc-c++ \
    geos-devel proj-epsg zlib-devel libjpeg-turbo-devel dejavu-sans-fonts

Prepare to NextGIS Web installation
------------------------------------

See  :ref:`ngw_install_prepare`.

NextGIS Web installation
-------------------------

Set environment variable ``PATH``:

.. code:: bash

    $ export PATH=/usr/pgsql-9.5/bin:$PATH

Install NextGIS Web in development mode. All required packages will be installed:

.. code:: bash

    $ env/bin/pip install -e ./nextgisweb

During packages build process there could occur a lack of memory. (was spotted 
on a computer with 512 MB of RAM during installation of ``lxml`` package), in 
this case you can increase memory size using a swap file,
`see more <http://stackoverflow.com/a/18335151/813758>`_.

Install MapServer
-------------------

MapServer is absent from repository of CentOS 7 so we built required packages:
`mapserver <http://nextgis.ru/programs/centos7/mapserver-7.0.0-1.el7.centos.x86_64.rpm>`_ 
and `mapserver-python <http://nextgis.ru/programs/centos7/mapserver-python-7.0.0-1.el7.centos.x86_64.rpm>`_.

Download them and install:

.. code:: bash

    $ sudo yum install fribidi cairo fcgi harfbuzz httpd librsvg2
    $ sudo rpm -ivh mapserver-7.0.0-1.el7.centos.x86_64.rpm
    $ sudo rpm -ivh mapserver-python-7.0.0-1.el7.centos.x86_64.rpm


After that you need to copy required files to a virtual environment directory used for NextGIS Web.

.. code:: bash

    $ mkdir env/lib/python2.7/site-packages/mapscript.egg
    $ cp /usr/lib64/python2.7/site-packages/*mapscript* \
    env/lib/python2.7/site-packages/mapscript.egg
    $ echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

Create a file ``PKG-INFO``:

.. code:: bash

    $ mkdir env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO
    $ touch env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO

Set the version of MapScript to be used:

.. code:: bash

    $ echo `python -c "import mapscript; print \
    'Version: %s' % mapscript.MS_VERSION"` \
    > env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO

For next steps see instructions if section :ref:`ngw_mapserver_install` 
and further.
