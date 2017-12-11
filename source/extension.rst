.. sectionauthor:: Natalia Baryshnikova <nshelekhova@gmail.com>

.. _ngw_extension:

Extensions
================================

General Information
-------------------

The following extensions are available to increase the functionality of the software:

1. Basemaps - added support for default basemap selection and a list view of available basemaps,
   an option to select from pre-prepared list of services.

2. Exporter - support for additional vector layer exporters to different formats (ESRI shapefile, MapInfo tab etc.)

3. File buckets - added support for file sets, ability to upload any files, including non-geospatial files.

4. Filtering - added support for filtering by attribute and spatial extent.

5. Mapnik - added support for Mapnik map renderer.

6. QGIS - added support for QGIS map renderer.

7. MapServer - added support for MapServer map renderer.

8. Logging - added support for writing of various messages to a log.

9. Mobile debug - added support for recieving and categorizing debug 
   information from NextGIS Mobile SDK.

10. Data editing - ability to edit geometries right in the browser.

11. Vector tiles - support for MVT and associated styles.
 
Adding a new extension
----------------------

Example: adding Rekod extension. 
Here are steps to add an extension:

1. Execute command to download extension source code to server where NextGIS Web is installed: 

.. code-block:: bash

   git clone https://github.com/nextgis/nextgisweb_rekod.git

2. Install extension in NextGIS Web: 

.. code-block:: bash

   env/bin/pip install -e ./nextgisweb_rekod

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
