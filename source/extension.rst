.. sectionauthor:: Natalia Baryshnikova <nshelekhova@gmail.com>

.. _ngw_extension:

Extensions
================================

General Information
--------------------

The following extensions are available to increase the functionality of the software:

1. Rosavto - added support for default basemap selection and a list view of available basemaps,
   an option to select from pre-prepared list of services.

2. Compulink - key - value dictionary added to NextGIS Web standard features,
   added support for vector layer export to different formats (ESRI shapefile, MapInfo tab etc.)

3. Rekod - added support for file sets, ability to upload any files, including non-geospatial.

4. Forest violations - added support for filtering by attribute and spatial extent.

5. Mapnik - added support for Mapnik map renderer.

6. QGIS - added support for QGIS map renderer.

7. MapServer - added support for MapServer map renderer.

8. Log - added support for writing of various messages to a log.

9. Mobile debug - added support for recieving and categorizing debug 
   information from NextGIS Mobile SDK.
 
Adding a new extension
------------------------------------

Example: adding Rekod extension. 
Here are steps to add an extension:

1. Execute command to download extension source code to server where NextGIS Web is installed: 

.. code:: bash

   git clone https://github.com/nextgis/nextgisweb_rekod.git

2. Install extension in NextGIS Web: 

.. code:: bash

   env/bin/pip install -e ./nextgisweb_rekod

3. Create nessesary directories (optional):

.. code:: bash

   mkdir file_bucket

4. Make changes to configuration file (optional):

.. code:: bash

   nano config.ini

Example: changes to configuration file:

.. code:: bash

   [file_bucket]
   path =  /home/username/ngw/file_bucket

5. Reinitialize NextGIS Web database:

.. code:: bash

   env/bin/nextgisweb --config config.ini initialize_db 

6. Restart NextGIS Web to apply changes.
