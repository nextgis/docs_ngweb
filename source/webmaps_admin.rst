
.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>

.. _ngw_webmaps_admin:

Administration of web map
===========================

There could be several web maps in NextGIS Web. For example one map is for work, another is for public access and third is for testing of layer styles.

Web maps have their own URL addresses. You can manage display web map access permission for different users and groups. 

Web map displays layers. User can turn web maps on and off. You can set the order for layers, define layers default visibility, create group layers. Groups on a map do not relate to groups of layers in control panel. There is no option to transfer layers between groups. Groups may be nested.

.. _ngw_map_create:
    
Creation of web map
---------------------

To add a web map navigate to a resource group where you want to create a map and in actions pane "Create resource" select "Web map" (see :numref:`ngweb_admin_layers_create_webmap`). 

.. figure:: _static/admin_layers_create_webmap_eng.png
   :name: ngweb_admin_layers_create_webmap
   :align: center
   :width: 16cm

   Selection of “Web map” action. 
   
Create resource dialog for a web map will open and will look like :numref:`admin_webmap_name`. 

.. figure:: _static/admin_webmap_name_eng.png
   :name: admin_webmap_name
   :align: center
   :width: 16cm

   Create resource dialog for web map.

Enter web map display name that will be visible in administrator interface and in map layer tree.

Field “Keyname” is optional.

You can also add resource description and metadata on the corresponding tabs.

Switch from “Resource” tab to “Extent and bookmarks” tab, which is presented on :numref:`admin_webmap_bbox`.

.. figure:: _static/admin_webmap_bbox_eng.png
   :name: admin_webmap_bbox
   :align: center
   :width: 16cm

   "Extent and bookmarks" tab.

Set the extent in four fields by setting degrees.

The field "Extent from layer" allows to set web map extent on the layer extent. If you click the icon with plus on a folder near this field, the window "Select resource" will open, where you can select a layer to use for setting web map extent (four fields with extent coordinates will be filled up, see :numref:`ngw_select_resource2`). 

.. figure:: _static/ngw_select_resource2_eng.png
   :name: ngw_select_resource2
   :align: center
   :width: 16cm

   "Select resource" window.

.. tip::
   Coordinates for extent could be generated using third-party services http://boundingbox.klokantech.com/ (select csv in a list), http://lxbarth.com/bbox.

You can select a vector layer for bookmarks in the field "Bookmark resource". If layer is set the client will 
have a menu with bookmarks with names defined by "Label attribute". 

Navigate to "Layers" tab. This tab is used for adding layer, joining 
them to groups or removing them. These actions could be performed using corresponding buttons 
"Add layer", "Add group" and "Remove" (see :numref:`admin_webmap_layers`). 

.. figure:: _static/admin_webmap_layers_eng.png
   :name: admin_webmap_layers
   :align: center
   :width: 16cm

   "Layers" tab.

When a layer is added you set its style. The style is connected with the layer, so that "Style" is absent in main resource group. To create a style navigate to the layer resource and in actions pane select "MapServer style" or "QGIS style". 
In opened window you can import a QML style from QGIS or enter the style manually (see :numref:`ngweb_layer_properties_window_pic`). 

Settings for layers on web map are presented on :numref:`admin_webmap_create_layers`.

.. figure:: _static/admin_webmap_create_layers_eng.png
   :name: admin_webmap_create_layers
   :align: center
   :width: 16cm
   
   Layers tab of create resource dialog.
 
A checkbox "Enabled" sets default visibility of a layer.

Field "Transparency" sets layer transparency on a map from 0% to 100%. A layer with 1% transparency is almost transparent, a layer with 100% transparency is opaque.

Field "Adapter" is recommended to be set to "Tiles" (256 x 256 pixels images) if there is no special requirements. You can also set it to "Image", so there will be one image on a whole map. 

For "Scale" parameters you may enter a scale range in the format like "1 : 10 000". Scale range could be set in a layer style and in layer settings. If layer should be displayed in a range of scales it is better to set this in layer properties, instead of a style.
   
.. note:: 
   If you set a scale range only in style   
   empty tiles will be generated for distribution which is less efficient.

"Layer order" button opens a window where you can set layer order different from layer order in layers tree to show on a map.

"Basemaps" tab allows to add and remove basemaps using corresponding buttons "Add" and "Remove" (see :numref:`admin_webmap_basemaps`). 

.. figure:: _static/admin_webmap_basemaps_eng.png
   :name: admin_webmap_basemaps
   :align: center
   :width: 16cm

   "Basemaps" tab.

A checkbox "Enabled" sets default visibility of a basemap.

Field "Transparency" sets basemap transparency on a map from 0% to 100%. A basemap with 1% transparency is almost transparent, a basemap with 100% transparency is opaque.

After a creation of map content and adjusting settings for all layers click a "Save" button. A saved map will be shown in a list of maps. 
Click an icon with a map in a list of web maps or click a "Display" link in a window with web map properties to open a web map in a client application. Web map client application is described in section :ref:`ngw_webmaps_client`.
Address which is used for web client may be transfered to other users because it is static. 

.. warning::  
   Address will become inavailable if a web map is deleted.

