.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>
.. sectionauthor:: Dmitry Baryshnikov <dmitry.baryshnikov@nextgis.ru>

.. _ngw_quick_tutorial:


Quickstart tutorial
===================

Quickstart tutorial for qualified users
-------------------------------------------------

**NextGIS Web** - is a server :term:`geographical information system` (:abbr:`GIS 
(geographical information system)`), which allows to display maps in web browser. 
Also NextGIS Web can share geodata :term:`geodata <geodata>` with other NextGIS 
software.

NextGIS Web has the following features:

* Display of maps in web browser (different maps with different layers and  
  styles)
* Flexible access permissions management
* Load of data from :term:`PostGIS`,of import from  
  files in :abbr:`GIS (geographical information system)` formats 
  :term:`ESRI Shape`,  :term:`GeoJSON` or :term:`GeoTIFF`
* Vector data could be downloaded in the following formats: :term:`GeoJSON`, 
  :term:`CSV`, :term:`ESRI Shape`
* Map styles could be imported from :term:`QGIS` project or could be set manually
* Could act as a server for :term:`TMS`, :term:`WMS`, :term:`WFS`
* Could act as a client for :term:`WMS`
* User can :ref:`add photos to records <ngw_add_photos>`, 
  :ref:`change records attributes <ngw_attributes_edit>`, :ref:`with a support  
  for a WFS-T protocol for editing <ngw_wfs_service>`.
* Listed features are available through a REST API from external software

NextGIS Web - is an open source software (license GPL v2+, see :ref:`ngw_gplv2`).

.. only:: html

   System requirements listed in section ":ref:`ngw_sys_req`".
   
.. only:: latex
   
   System requirements listed in `section http://docs.nextgis.com/docs_ngweb/source/general.html#recommended-hardware`_.


Integration with other NextGIS software
----------------------------------------

.. only:: html

   To manage :term:`geodata <geodata>` in NextGIS Web you can use 
   desktop application :ref:`NextGIS Manager <ngman_intro>`. 

.. only:: latex

   To manage :term:`geodata <geodata>` in NextGIS Web you can use 
   desktop application `NextGIS Manager <http://docs.nextgis.ru/docs_ngmanager/source/intro.html>`_. 

This software simplifies batch processing of data in NextGIS Web.

.. only:: html

   Mobile application :ref:`NextGIS Mobile <ngmob_intro>` allows to upload 
   geodata collected in the field directly to web gis in online or offline mode. 

.. only:: latex

   Mobile application `NextGIS Mobile <http://docs.nextgis.ru/docs_ngmobile/source/intro.html>`_ allow to upload 
   geodata collected in the field directly to web gis in online or offline mode. 
   
   
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
2. Add layers from created connection
3. Create styles for added layers

To create a raster layer follow these steps:

1. Prepare a raster file to meet the requirements
2. Create a raster layer from a file through administrator interface
3. Add a style to raster layer


After layers are loaded they may be added to a web map. Layers could be added in web map properties page. Then a web map could be displayed by address.

If there are many layers to add this operation will be easier performed with :ref:`NextGIS Manager <ngman_intro>`.

