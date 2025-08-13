.. _collector:

.. _nextgis.com: http://nextgis.com/
.. _NextGIS Collector: https://play.google.com/store/apps/details?id=com.nextgis.collector

Collector projects
======================================

.. note::
    You can use described functionality in Web GIS created in nextgis.com_ service on `Premium plan <https://nextgis.com/pricing-base/>`_

.. _ngw_collector_add_members:

List of participants
-------------------------------

In the Collector Projects section of the Control Panel, you can manage the list of `data collectors <https://docs.nextgis.com/docs_ngcom/source/collector.html>`_. Each participant must have a `NextGIS ID account <https://docs.nextgis.com/docs_ngcom/source/create.html#how-to-create-account-nextgis-id>`_. 


.. figure:: _static/ngc-stages-004_eng_2.png
   :name: list_of_collectors_empty_pic
   :align: center
   :width: 20cm

   List of collectors

To add a team participant to the Web GIS press "Create" button.
It will redirect you to the "Create new collector" page. Make sure to type in full email address that serves as NextGIS ID login. 

.. note::
    We recommend filling up the field "Description" with the name and the surname of the team participant in order to have data about all NextGIS Collector users in one place. 
    You can always find the participant you need with a search tool in a table of Collector users, which is quite suitable when there are a lot of participants. 
 

.. figure:: _static/ngc-stages-005_eng_2.png
   :name: create_collector_pic
   :align: center
   :width: 20cm

   Creating a new data collection participant


As a result of this stage all data collection team participants will be registered in your Web GIS.

.. figure:: _static/ngc-stages-006_eng_2.png
   :name: list_of_collectors_pic
   :align: center
   :width: 20cm

   An example of a filled list of collectors

Users with a registration in your Web GIS can access data collection projects from your Web GIS and begin data collection after they installed the `NextGIS Collector <https://play.google.com/store/apps/details?id=com.nextgis.collector>`_ mobile app and successfully sign in there. 

However you can control the access of different users to each individual project. 
It is described in details below.

.. _ngw_collector_create_project:

Creating data collection project
---------------------------------

Data collection project is a resource in your Web GIS, it is a set of layers for editing. 
In a Web GIS "data collection project" is called "Collector Project".
Data collection project allows a data collection team participant to edit its layers. 
Web GIS owner can restrain access to the project for separate participants. 

You can create a Collector project via NextGIS Formbuilder (the simplest way, described `here <https://docs.nextgis.com/docs_formbuilder/source/workflow.html#nextgis-web>`_) or in your Web GIS.

If you want to use your Web GIS to create a Collector project, first you need to `create necessary data layers <https://docs.nextgis.com/docs_ngweb/source/layers.html#ngw-create-empty-vector-layer>`_  or `upload them from a file <https://docs.nextgis.com/docs_ngweb/source/layers.html#ngw-process-create-vector-layer>`_.

Let's suppose that layers with data are already uploaded to your Web GIS, and you want to create a project and allow data collection team participants to collect or edit data in your Web GIS. 

To do it:

1. Open the Web GIS.

2. Create a basemap if the collector will need to see a map on the mobile app.

3. Press «Create resource» and select «Collector project»:

.. figure:: _static/select_create_collector_project_en.png
   :name: create_collector_project_pic
   :align: center
   :width: 20cm

   Select «Collector project»

4. Name your project. This name will be displayed in the `NextGIS Collector`_ mobile app :

.. figure:: _static/ngc_proj_name_en.png
   :name: ngc_proj_name_pic
   :align: center
   :width: 20cm

   Adding name for Collector project


In the "Project" tab select "Starting screen" and fill in "NextGIS Collector user credentials". 


The starting screen in the `NextGIS Collector`_ mobile app could be a list of forms or a map. 


«NextGIS Collector user credentials» - user name and password of a Web GIS user with necessary permissions to access data used in the project. This user is not related to accounts of actual data collectors.


.. figure:: _static/ngc_proj_tab_en.png
   :name: ngc_proj_tab_pic
   :align: center
   :width: 20cm

   "Project" tab

6. On the Items tab you can Add items, Group them, Delete (X symbol on the right) and change the order by dragging items in the list. 

Items that can be added to a Collector project are: editable vector layer, display-only layer, basemap or data collection form.


Click "+ Layer" to add an item.

* To add an **editable data layer** select the *layer* (If a layer has two or more forms, you can select one or several of them);
* To add a **display-only layer** select its *style*;
* To add a **basemap** select the resource.

You can add multiple items at once.

Items selected to be added are marked with a tick. A layer that has a style or form(s) selected is marked with a blue dot. 

Navigate between resource groups and tick the items you want to add. The **Add selected** button displays the total number of selected resources. To clear the selection press the  |button_clear_selection| button next to it.

.. |button_clear_selection| image:: _static/button_clear_selection.png
   :alt: X
   :width: 8mm 

.. figure:: _static/ngc_items_tab_add_en.png
   :name: ngc_items_tab_add_pic
   :align: center
   :width: 20cm

   Adding several items to a project. Four items selected: a basemap, a layer, a form and a style

To add display-only layer you don't need to open it and select the style.  Press the |button_pick_first| button to the right of the layer name to auto-select the first style.

.. |button_pick_first| image:: _static/button_pick_first.png
   :alt: editor's choice
   :width: 8mm

Press **+ Group** to create a group of items. 
Drag-and-drop to rearrange items within the item tree. To delete an item, press **X** at the end of the row. 

Click on the item to see its attributes.

.. figure:: _static/ngc_items_tab_en.png
   :name: ngc_items_tab_pic
   :align: center
   :width: 16cm

   "Items" tab

Each item of Collector project has the following attributes:

- «Display name» - a layer name which is displayed in the NextGIS Collector mobile app. 
- «Editable» - allow or deny editing of the layer in the NextGIS Collector mobile app.  
- «Visible» - controls layer's visibility in the NextGIS Collector mobile app. 
- «Syncable» - allow or deny synchronization of the layer with your Web GIS.
- «Zoom level visibility» - defines for which zoom levels the layer is visible. It has two parameters: Min zoom and Max zoom.
- «Lifetime for tiles (in min)» - time of tiles cashing (for tile layers).

To go back to the list of items, press **Hide details**.

7. Then on the "Collectors" tab tick the users participating in the project to give them permissions: 


.. figure:: _static/ngc_list_collectors_en.png
   :name: ngc_list_collectors_pic
   :align: center
   :width: 16cm

   «Collectors» tab

8. Press "Create".

As a result a Collector project (data collection project) will be created.

You can have unlimited number of projects in your Web GIS. In each of them you can restrain or allow access for a particular set of users from the data collection participants list.

.. _collector_create_form:

Data collection form
---------------------

For a vector layer you can create a data collection form as a child resource.

Open the resource page of the layer, click **Create resource** and select "Form".

.. figure:: _static/ngweb_create_form_en.png
   :name: ngweb_create_form_pic
   :align: center
   :width: 20cm

   Selecting "Form" resource type

In the opened window on the Form tab you have two options:

* build a form;  
* upload a NGFP file made in Formbuilder.



To create a new form in the online builder, drag the elements from the list on the left to the middle field. Click on the element to modify it and select the field in which this data will be stored.

.. figure:: _static/form_build_en.png
   :name: form_build_pic
   :align: center
   :width: 20cm

   Building a form online. Properties of the "Text box" element are displayed

If you tick **Add absent fields to layer**, fields for the added elements will be added automatically. This allows users to create an empty layer, then set its structure by creating a form.

You can set a display name on the Resource tab and add description and metadata on the corresponding tabs.

Form can be **edited**. Press the pencil icon next to it or open the resource page and click **Edit**. If the form was uploaded from a file, on the Form tab select Design form from the dropdown menu.

Alternatively, you can edit the form in Formbuilder and upload a new file.

.. figure:: _static/form_upload_en_2.png
   :name: form_upload_pic
   :align: center
   :width: 20cm

   Uploading form file

You can have **multiple** forms for one layer. Include different forms in different Collector projects or add several forms for one layer in one project. 

After a form is modified, select "Change project" and re-join the project. The new form will be uploaded, allowing you to continue collecting data to the same layer.

.. note:: Data collection form can be uploaded to Web GIS using `Formbuilder <https://docs.nextgis.com/docs_formbuilder/source/workflow.html>`_. A vector layer is created automatically. Optionally, you can add a Web Map and a Collector project. 
