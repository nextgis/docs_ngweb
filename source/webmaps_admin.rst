
.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>

.. _ngw_webmaps_admin:

Administration of web map
===========================

There could be several web maps in NextGIS Web. For example one map is for work, another for public access and third for testing of layer styles.

Web maps have their own URL addresses. You can manage display web map access permission for different users and groups. 

Web map display layers. User can turn them on and off. You can set the order for layers, define layers default visibility, create group layers. Groups on a map does not relate to groups of layers in control panel. There is no option to transfer layers between groups. Groups may be nested.


.. _ngw_map_create:
    
Creation of web map
---------------------

To add a web map navigate to a resource group where you want to create a map. In Actions pane select :menuselection:`Create resource --> Web map`. Create resource dialog for a web map is presented on :numref:`admin_webmap_name`. 

.. figure:: _static/admin_webmap_name.png
   :name: admin_webmap_name
   :align: center
   :width: 16cm

   Create resource dialog for web map.


Enter web map display name that would be visible in administrator interface and in map layer tree.
Fields :guilabel:`Keyname` and :guilabel:`Description` are non-required.

Switch from :guilabel:`Resource` tab to :guilabel:`Extent and bookmarks`. 
A tab for Extent and bookmarks will open:numref:`admin_webmap_bbox`.

.. figure:: _static/admin_webmap_bbox.png
   :name: admin_webmap_bbox
   :align: center
   :width: 16cm

   An Extent and bookmarks tab of create resource window.

Setup an extent.

.. note:: 
   Coordinates for extent could be generated using third-party services http://boundingbox.klokantech.com/ (select csv in a list), http://lxbarth.com/bbox.

You can select a vector layer for bookmarks. If layer is set the client will 
have a menu with bookmarks with names defined by :guilabel:`Label attribute`. 

Navigate to :guilabel:`Layers` tab. This tab is used for adding layer joining 
them to groups. These actions could be performed using corresponding buttons 
:guilabel:`Add layer` and :guilabel:`Add group`.

When a layer is added you set its style. Settings for layers on web map are 
presented on :numref:`admin_webmap_layers`..


.. figure:: _static/admin_webmap_layers.png
   :name: admin_webmap_layers
   :align: center
   :width: 16cm
   
   Layers tab of create resource dialog.
 
A checkbox :guilabel:`Enable` sets default visibility of a layer.

Field :guilabel:`Adapter` is recommended to be set to :guilabel:`Tiles` if there is no special requirements.

For :guilabel:`Scale` parameters you may enter a scale range in the following format :guilabel:`1 : 10 000`.

Scale range could be set in a layer style and in layer settings. If layer should be displayed in a range of scales it is better to set this in layer properties, instead of a style.
   
.. note:: 
   If you set a scale range only in style   
   empty tiles will be generated for distribution which is less efficient.

After a creation of map content and adjusting settings for all layers click a :guilabel:`Save` button. A saved map will be shown in a list of maps. 
In a list of web maps click a :guilabel:`Display` link to open a web map in a client application. Web map client application is described in section :ref:`ngw_webmaps_client`.
Address which is used for web client may be transfered to other users because it is static. 

.. note:: 
   Address will become inavailable if a web map is deleted.

