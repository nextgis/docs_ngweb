

.. _ngw_src:

Spacial Reference Systems
=========================

You can manage spacial reference systems using Control panel. It allows browsing the list of added SRS, importing SRS from the catalog and creating your own.

.. figure:: _static/Control_panel_SRS_en.png
   :name: Control_panel_SRS_pic
   :align: center
   :width: 10cm
   
   Spacial reference systems in the Control panel

To see which SRS are already added, select "List".  In this list there are two SRS by default: «WGS 84 / Lon-lat (EPSG:4326)» and «WGS 84 / Pseudo-Mercator (EPSG:3857)».


Custom Spacial Reference Systems
----------------------------------

.. note::
    Custom SRS functionality is available on `Premium <https://nextgis.com/pricing-base/>`_ and while using the product `On-premise <https://nextgis.com/pricing/>`_.

.. note::
    Only users with administrative permissions can add and modify SRS.
    
If needed, you can import additional SRS from the catalog or create your own.

Import SRS from catalog
~~~~~~~~~~~~~~~~~~~~~~~~

To add an SRS from the catalog, go to the Control panel and in the Spacial reference system section press **Catalog** (or, if you are viewing the SRS list, press **Import from catalog**). 
You will be redirected to the catalog page. Start typing the name of the SRS in the search bar. When you find the desired SRS in the search results, press the icon with the arrow next to it.

.. figure:: _static/new_srs_catalog_en.png
   :name: new_srs_catalog
   :align: center
   :width: 20cm    

   Search results in the catalog
   
You will be redirected to the import page. Here you can modify the name of SRS to be displayed in your WebGIS.

.. figure:: _static/new_srs_import_en.png
   :name: new_srs_import
   :align: center
   :width: 20cm    

   Adding SRS from catalog
   
.. figure:: _static/new_srs_import_save_en.png
   :name: new_srs_import_save
   :align: center
   :width: 20cm    

   Completing import
   
On the next page, press **Save** to complete the import.


Creating new SRS
~~~~~~~~~~~~~~~~~

To create a new SRS open “Control panel” in the main menu and press **Create** in “Spatial reference systems” (or press **Create** while on the SRS list page).

.. figure:: _static/new_srs_eng_2.png
   :name: new_srs_pic
   :align: center
   :width: 20cm    

   Creation of a new SRS
   
You can give an SRS display name and enter its definition in OGC WKT format.  You can also import definitions from common formats as PROJ, MapInfo and EPSG, after the import they will get converted to OGC WKT format.  Then press **“Create”** .
The new SRS will appear in the list. 


How to edit or delete SRS
----------------------------

You can change the *display name* of any SRS by clicking the pencil icon in the corresponding row of the list. If you created an SRS yourself, you can also modify its *OGC WKT definition*.

.. figure:: _static/list_srs_edit_en.png
   :name: list_srs_edit_pic
   :align: center
   :width: 16cm 
   
   Editing icon in the SRS list

.. figure:: _static/srs_edit_en.png
   :name: srs_edit_pic
   :align: center
   :width: 16cm 
   
   Editing SRS

To *delete* an SRS, press the trash can icon in the corresponding row of the list. A small dialog window will appear. Confirm that you want to delete the SRS by pressing **OK**.

.. figure:: _static/delete_srs_en.png
   :name: delete_srs_pic
   :align: center
   :width: 20cm 
   
   Deleting custom SRS

After the operation is completed, a message will appear announcing "SRS deleted".
The two default SRS, «WGS 84 / Lon-lat (EPSG:4326)» and «WGS 84 / Pseudo-Mercator (EPSG:3857)», can’t be removed.


Usage of additional SRS
-------------------------------

The added SRS can be used for various purposes:

1. To capture coordinates on Web maps. If you have set up additional SRS (one or several), you can now conveniently capture coordinates in this SRS from anywhere on the map: 
   
2. To export vector layers.  All custom SRS are also available for data export. See `this section <https://docs.nextgis.com/docs_ngweb/source/admin_interface.html#data-export-to-csv-and-geojson-formats>`_ for details.

3. To extend API requests. Support for custom SRS is gradually added to NextGIS Web API too.  For example, this request will return a feature in a required SRS (in example we use custom SRS identifier 990002)

/api/resource/{id}/feature/{fid}?srs=990002


.. _ngw_srs_id:

SRS identifier
~~~~~~~~~~~~~~~~~

For API or some Toolbox tools (e.g. `Web Map to QGIS project <https://toolbox.nextgis.com/t/webmap2qgis>`_) you need to know the identifier of the SRS.

If it's a standard SRS that has **EPSG** code, just use the numbers of the code, e.g. ``3857`` for EPSG:3857.

If it's a custom SRS `added via definition <https://docs.nextgis.com/docs_ngweb/source/ngw_srs.html#ngw-srs-create>`_ that does not has EPSG code, it gets a unique code within a specific Web GIS. To view it, open the SRS in the edit mode, the identifier is at the end of the URL:

.. figure:: _static/user_srs_id_en.png
   :name: user_srs_id_pic
   :align: center
   :width: 20cm 

   Identifier of a custom SRS

.. _ngw_srs_external_db:

Custom SRS support for external PostGIS Databases
-------------------------------------------------

A common case for Web GIS users is adding an external PostGIS/PostgreSQL database while `creating a PosGIS layer <https://docs.nextgis.com/docs_ngcom/source/data_connect.html#external-postgis-databases>`_. These layers often get incorrectly displayed 
in the Web GIS. It happens when the spacial reference system has incorrect definition in the external 
database. To make the Web GIS-DB complex work efficiently, here's what you need:

1. External database must have a table of SRS descriptions spacial_ref_sys.
2. In the geometry column (usually called "geom") a SRS must be assigned.
3. The ID of the assigned SRS must be included in  spacial_ref_sys.
4. Data in the external DB must actually be in that coordinate system, a.i. the SRS definition must correspond to the data.

If all the above requirements are met, then whatever SRS you use in your database, layers created in Web GIS will be reprojected "on the fly" and displayed correctly along any other data you have in your Web GIS.

To check if everything works correctly use `PostGIS diagnostics <https://docs.nextgis.com/docs_ngweb/source/postgis_details.html#postgis-diagnostics>`_.
