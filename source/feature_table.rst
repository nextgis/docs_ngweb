Managing Feature table
========================

Feature table allows to perform the following operations with a selected feature  (see :numref:`admin_table_objects1_upload`):

1. Open
2. Edit (in a new tab or in the same tab)
3. Delete
4. Save as (advanced or quick export available)
5. Use Search Box
6. Refresh the table
7. Open table settings



.. figure:: _static/table_objects1_eng_3.png
   :name: admin_table_objects1_upload
   :align: center
   :width: 16cm

   Actions for the selected feature in the feature table

There is another way to open Feature table. In the adminitrative interface navigate to a child resource group where resource types are marked and find a resource with a type Web Map. Open it by clicking on the "Display" icon (see :numref:`webmap_open_from_group_pic`):

.. figure:: _static/webmap_open_from_group_en.png
   :name: webmap_open_from_group_pic
   :align: center
   :width: 20cm

   Opening a Web Map from the list

Alternatively, you can go to the resource page and click "Display" in the Web Map actions pane on the right.

.. figure:: _static/webmap_open_from_page_en.png
   :name: webmap_open_eng_pic
   :align: center
   :width: 20cm

   Opening a Web Map from the resouce page

A Web Map will be opened with a layer tree (left) and a map (right). To view a feature table select the required layer in layer tree and then select "Feature table" command in the Layer drop down menu at the top of layer tree :numref:`admin_map_and_tree_layers_upload`:
  
Table allows to perform the following operations with the selected feature  :numref:`admin_table_objects2_upload`:

1. Open in a new tab
2. Edit 
3. Delete
4. Go to (after a click the selected feature will be displayed on the map)
5. Save as (advanced or quick export available)
6. Zoom to filtered features
7. Filter features by area
5. Use Search Box
6. Refresh the table
7. Open table settings

 
.. figure:: _static/table_objects2_eng_3.png
   :name: admin_table_objects2_upload
   :align: center
   :width: 20cm

   Actions for the selected record in feature table

You can also `edit the attributes <https://docs.nextgis.com/docs_ngweb/source/edit_resource.html#edit-vector-layer-attributes-table>`_ themselves.


.. _ngw_feature_table_filter_area:

Filter layer features on the Web Map by area
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NextGIS Web has a tool in the Feature table that filters all layer features within a selected area. To choose area limits just draw them on the Web Map.

Open the feature table and click on the button with a dotted frame. In the dropdown menu select the geometry of the area:

* circle (click twice on the map, to choose the center of the circle and its size, the radius length is shown in meters)
* line (features intersected by the line will be filtered)
* rectangle (click on diagonally opposite apexes)
* free-hand drawn polygon (each click creates an apex, the area covered by the polygon is highlighted; to finish the shape, double-click on an apex, the polygon will be completed automatically)

.. figure:: _static/ngweb_filter_by_area_geometry_en.png
   :name: ngweb_filter_by_area_geometry_pic
   :align: center
   :width: 20cm

   Selecting filter geometry

Now the feature table only contains the features within the selected area. The tool button will have the current area shape on it. In the dropdown menu you can use one of the following options:

* Show/Hide the outline and fill of the selected area
* Zoom to the filtering area
* Clear filtering geometry

.. figure:: _static/ngweb_filter_by_area_actions_en.png
   :name: ngweb_filter_by_area_actions_pic
   :align: center
   :width: 20cm

   Filter actions

You can use quick export to save the filtered features in a variety of common geodata formats. Click **Save as** and select in the dropdown menu Quick export with default settings or Advanced export to modify parameters (see detailed description below).
