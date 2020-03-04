.. sectionauthor:: Natalia Baryshnikova <nshelekhova@gmail.com>

.. _ngw_extension:

Extensions
==========

General Information
-------------------

The following extensions are available to increase the functionality of the software:

1. **Basemaps** - adding and managing basemaps.

2. **File buckets** - file sets, adds ability to upload any files, including non-geospatial ones.

3. **QGIS** - map rendering with QGIS.

4. **MapServer** - map rendering with MapServer.

5. **Audit** - logging user events such as log in/out, resource addition or removal etc.

6. **NextGIS ID on premise** - enterprise authentification and authorization features, such as LDAP or OAuth.

Specific extensions may require installation of additional software components.

 
Installing an extension
-----------------------

Here are the steps to add nextgisweb_filebucket extension:

1. Execute command to download extension source code to a server where NextGIS Web is installed: 

.. code-block:: bash

   git clone https://github.com/nextgis/nextgisweb_filebucket.git

2. Install extension in NextGIS Web: 

.. code-block:: bash

   env/bin/pip install -e ./nextgisweb_filebucket

3. Create nessesary directories (optional):

.. code-block:: bash

   mkdir file_bucket

4. Make changes to configuration file (optional):

.. code-block:: bash

   nano config.ini

Example: changes to configuration file:

.. code-block:: bash

   [file_bucket]
   path =  /home/username/ngw/file_bucket

5. Reinitialize NextGIS Web database:

.. code:: bash

   env/bin/nextgisweb --config config.ini initialize_db 

6. Restart NextGIS Web to apply changes.

.. _`ngw_install_qgis`:

Adding NextGIS Web QGIS extension
----------------------------------
This extension adds support for rending with QGIS. To operate it needs QGIS 2.8 or higher already installed on the system.

.. code-block:: bash

    cd ~/ngw
    git clone git@github.com:nextgis/nextgisweb_qgis.git
    source env/bin/activate
    pip install -e nextgisweb_qgis/

QGIS and PyQT4 dependencies are not listed in ``setup.py`` because it is hard to install it in virtualenv. So lets copy these libraries from system packages to virtualenv. On Ubuntu these libraries are located in ``python-sip``, ``python-qt4`` and ``python-qgis`` packages.

.. code-block:: bash

    # DST should point to virtualenv site-packages directory.
    # If it is point to another place you have to modify DST definition.
    # For example: DST=`python -c "import sys; print sys.path[-2]"`
    DST=`python -c "import sys; print sys.path[-1]"`
    echo $DST
    cp `/usr/bin/python -c "import sip; print sip.__file__"` $DST
    cp -r `/usr/bin/python -c "import PyQt4, os.path; print os.path.split(PyQt4.__file__)[0]"` $DST
    cp -r `/usr/bin/python -c "import qgis, os.path; print os.path.split(qgis.__file__)[0]"` $DST

For QGIS 2.16 and higher:

.. code-block:: bash

    # Only for latest QGIS version (2.16 and higher)
    export PYTHONPATH=$PYTHONPATH:/usr/share/qgis/python
    cp -r `/usr/bin/python -c "import PyQt, os.path; print os.path.split(PyQt.__file__)[0]"` $DST
    
uWSGI Deployment Notes
~~~~~~~~~~~~~~~~~~~~~~

You have to add `--lazy-apps` to the command line, in this way application will be loaded after master's fork, so each worker will get its thread.

.. code-block:: bash

   [uwsgi]
   lazy-apps = True

Beware as there is an older options named `lazy` that is way more invasive and highly discouraged (it is still here only for backward compatibility).

If you get an error message `ERROR: Auth db directory path could not be created` then you have to specify directory where an existing qgis-auth.db is located or created if not present.
This directory needs to be writeable by uwsgi process user. For example:

.. code-block:: bash

   [uwsgi]
   env = QGIS_AUTH_DB_DIR_PATH=/var/www

If you don't see cyrillic labels add the following environment variable:

.. code-block:: bash

   environment = LC_ALL="en_US.UTF-8"
