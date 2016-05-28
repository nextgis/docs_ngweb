.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>

.. _ngw_change_layers:

Layer settings
================================

Introduction
-------------

Layers settings become available after a layer creation.

To edit a layer you need to open its layer properties page and select :guilabel:`Update` on :guilabel:`Actions` pane . Update resource page will open for editing layer properties. Also you can launch update from layers table using :guilabel:`Actions` column.

.. _ngw_attributes:

Layer attributes
-----------------

In addition to initial parameters that were entered during layer creation there is an :guilabel:`Attributes` tab (see  :numref:`admin_layers_attr`)


.. figure:: _static/admin_layers_attr.png
   :name: admin_layers_attr
   :align: center
   :width: 16cm

   :guilabel:`Attributes` tab of Update resource dialog.

The tab has a table with a list of all layer attributes.

* A checkbox in :guilabel:`FT` (feature table) column means that attribute will be visible in identify window.
* A checkbox in :guilabel:`LA` (label attribute) means that this attribute is used to provide a name for identify operation and a name for a bookmark list. 

You can set an alias for each attribute name to display it in identify window instead of an attribute name.


.. figure:: _static/webmap_identification.png
   :name: webmap_identification
   :align: center
   :width: 16cm

   Identify window.

.. _ngw_attributes_edit:

Edit attribute values
----------------------------------

NextGIS Web software allows to edit attributes for geographical features. Editing could be launched from administrator interface or from a map display. 

* Editing of attributes from administrator interface: open a vector layer, click on right 
  menu :guilabel:`Feature table`. A feature table for the layer will open. Click a radio 
  button at the left side of a row you want to edit and click :guilabel:`Edit` button.
* Editing of attributes from a map display: click on a map with  
  identify tool active and click edit button in identify window.

You can change attribute values in opened window. 
Description made on Description tab will be visible on a map display in indentify window. 

When editing a PostGIS layer attribute changes are saved to PostGIS database and descriptions are saved to a local database. 
When editing a layer based on a Shapefile attribute changes are saved to a local database. 

Geodata with changed attributes could be downloaded by link :guilabel:`Download as 
GeoJSON` or published as WFS service. Download of descriptions is currently unavailable.

.. _ngw_add_photos:

Adding photos to geodata
----------------------------------

NextGIS Web software support adding photos to feature records. 
If a feature has attached photos they will be shown with a description and attributes in identify window (see  :numref:`webmap_identification_photos`).

.. figure:: _static/webmap_identification_photos.png
   :name: webmap_identification_photos
   :align: center
   :width: 16cm

   Identify window with an identify result with attached photos.


You can add photos in a map display. 

.. note:: 
   By default photos could be added by any user but there is an option 
   to limit number of users who can upload photos (see  
   :ref:`ngw_access_rights`).

1. Click on a feature on a map with active Identify tool.
2. Click edit button in Identify window.
3. In the edit tab navigate to :guilabel:`Attachments` tab 
   (see  :numref:`admin_object_edit_attr`).

.. figure:: _static/admin_object_edit_attr.png
   :name: admin_object_edit_attr
   :align: center
   :width: 16cm

   Edit attributes tab.

4. Upload photos. JPEG and PNG formats are supported. GIF format is not supported (see. :numref:`admin_object_edit_photos1`).

.. figure:: _static/admin_object_edit_photos1.png
   :name: admin_object_edit_photos1
   :align: center
   :width: 16cm

   Attachments tab of edit feature window for upload of photos.

5. Click :guilabel:`Save`, enter Description and then one more time 
   click :guilabel:`Save`  (see  :numref:`admin_object_edit_photos2`).

.. figure:: _static/admin_object_edit_photos2.png
   :name: admin_object_edit_photos2
   :align: center
   :width: 16cm

   Attachment tab with uploaded photos with filled descriptions.

After upload of photos you can see previews of photos on :guilabel:`Attachments` tab if identify window (see  :numref:`webmap_identification_photos`).


After a click on a photo preview a lightbox window is open (a javascript powered window in browser). Photo size is adjusted to fit the window. Photos have descriptions and user can navigate through photos using left and right arrow keys on a keyboard (see  :numref:`webmap_identification_photo_lightbox`).

.. figure:: _static/webmap_identification_photo_lightbox.png
   :name: webmap_identification_photo_lightbox
   :align: center
   :width: 16cm

   A lightbox with uploaded photos for an identified feature.  


To delete a photo select it in a table on Attachments tab of edit feature page and click :guilabel:`Delete`, and then click :guilabel:`Save` button.

