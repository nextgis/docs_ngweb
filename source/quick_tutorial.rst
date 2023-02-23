.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>
.. sectionauthor:: Dmitry Baryshnikov <dmitry.baryshnikov@nextgis.ru>

.. _ngw_quick_tutorial:


Quickstart tutorial
===================

Quickstart tutorial for advanced users
-------------------------------------------------

**NextGIS Web** - is a server :term:`geographical information system` (:abbr:`GIS 
(geographical information system)`), which allows to store and to edit geodata and to display maps in web browser. 
Also NextGIS Web can share :term:`geodata <geodata>` with other NextGIS 
software.

NextGIS Web has the following features:

* Create and display maps in web browser (unlimited number of maps with different layers and styles)
* Flexible access management
* Geodata could be loaded from :term:`PostGIS` or imported from  
  files in :abbr:`GIS (geographical information system)` formats: :term:`ESRI Shape`,  :term:`GeoJSON`,  :term:`GeoTIFF`,  :term:`GeoPackage` and many others.
* Vector data could be downloaded in the following formats: :term:`GeoJSON`, :term:`CSV`, :term:`ESRI Shape`, :term:`DXF`, Mapinfo TAB, MIF/MID, GeoPackage.
* Map styles could be imported from :term:`QGIS` project or could be set manually
* Act as a server for :term:`TMS`, :term:`WMS`, :term:`WFS`
* Act as a client for :term:`TMS` and :term:`WMS`
* User can :ref:`add photos to records <ngw_add_photos>`, 
  :ref:`change records attributes <ngw_attributes_edit>`, :ref:`with a support  
  for a WFS-T protocol for editing <ngw_wfs_service>`.
* Listed features are available through a REST API from external software

.. only:: html

   System requirements listed in section ":ref:`ngw_sys_req`".
   
.. only:: latex
   
   System requirements listed in `section http://docs.nextgis.com/docs_ngweb/source/general.html#recommended-hardware`_.


Integration with other NextGIS software
----------------------------------------

To manage geodata in NextGIS Web you can use the specialized plugin for NextGIS QGIS - `NextGIS Connect <https://docs.nextgis.com/docs_ngconnect/source/toc.html>`_.

Use the specialized NextGIS Knnekt module to manage geodata in NextGIS Web.

This software simplifies batch processing of data in NextGIS Web.

.. only:: html

   Mobile application :ref:`NextGIS Mobile <ngmob_intro>` allows to upload 
   geodata collected in the field directly to Web GIS in online or offline mode. 

.. only:: latex

   Mobile application `NextGIS Mobile <http://docs.nextgis.ru/docs_ngmobile/source/intro.html>`_ allow to upload 
   geodata collected in the field directly to Web GIS in online or offline mode. 
   
   
Several mobile devices could see data changes in a single layer.

.. todo: Write about plugin for QGIS - NGW Admin


Map creation
--------------

To create a vector layer from :term:`ESRI Shape` follow these steps:

1. Compress a shapefile to a zip-archive
2. Create a vector layer using a zip-archive through administrator interface
3. Add a style to newly created vector layer

To create a vector layer from :term:`GeoJSON` follow these steps:

1. Create a vector layer from GeoJSON file through administrator interface
2. Add a style to newly created vector layer

To create a vector layer from :term:`PostGIS` follow these steps:

1. Add a PostGIS connection (user login and password are required for access to 
   database)
2. Add layers using created connection
3. Create styles for added layers

To create a raster layer follow these steps:

1. Prepare a raster file meeting the requirements
2. Create a raster layer from a raster file
3. Add a style to raster layer


After layers are loaded they should be added to a web map in web map properties page. Then a link to web map could be opened in a web browser.


