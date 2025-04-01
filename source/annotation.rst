.. _ngw_annotation:

.. _nextgis.com: http://nextgis.com/
.. _WYSIWYG: https://en.wikipedia.org/wiki/WYSIWYG
.. role:: raw-html(raw)
    :format: html

Web Map annotations
===================

Annotation. What is it?
~~~~~~~~~~~~~~~~~~~~~~~~

Annotations are text messages attached to the points, which you can create and display on any `Web Map <https://docs.nextgis.com/docs_ngweb/source/webmaps_admin.html>`_. You can create your own set of annotations for each Web Map.

Annotation consists of a point and a message attached to this point.

.. figure:: _static/ann_annotation_structure_new_eng.png
   :name: ann_annotation_structure_new_pic
   :align: center
   :width: 20cm

   Annotation structure (1 - a text message of annotation, 2 - a point of annotation)

The main aim of annotations is to specify user's data by placing temporary messages on a Web Map.

.. figure:: _static/ann_messages_example_eng_2.png
   :name: ann_messages_example
   :align: center
   :width: 20cm

   An example of annotation display

At the same time, you can use annotations as a simple tool to create point data with text attributes attached to the Web Map.

.. figure:: _static/ann_data_example_eng_2.png
   :name: ann_data_example
   :align: center
   :width: 20cm

   An example of annotation display (as point data)

.. note::
    In contrast to a full vector layer, annotation tool does not allow to export data, search for it etc. Therefore, we recommend using `vector layers <https://docs.nextgis.com/docs_ngweb/source/layers.html#empty-vector-layer>`_ to create the bulk of the data.

.. _ngw_annotation_enable:

How to enable Web Map annotations?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can enable creation of annotations and set the display options in the Settings tab of the "Create resource" or "Update resource" windows for the Web Map (see :ref:`Update resource <ngw_update_resource>`). By default the annotation tool is inactive.

.. figure:: _static/ann_settings_eng_3.png
   :name: ann_settings
   :align: center
   :width: 16cm

   Settings tab of a Web Map for managing annotations (annotations are enabled and are shown on a Web Map when it opens)

There are two parameters in the Settings tab of a Web Map:

**Enable annotations** - enable or forbid working with annotations while working with the Web Map.

**Show annotations**:

- *No* - annotations are hidden
- *Yes* - the annotation symbols are visible on the Web Map when it opens
- *With messages* - both symbols and text are shown on the Web Map

.. _ngw_annotation_panel:

Web Map: Annotations panel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the *"Enable annotations"* option is active, the "Annotations" panel appears on the Web Map:

.. figure:: _static/ann_panel_eng_3.png
   :name: ann_panel
   :align: center
   :width: 20cm

   "Annotations" panel on a Web Map

"Annotations" panel consists of several options:

**Show annotations** - allows to show or hide symbols and messages of annotations.

**Edit annotations** - activate or inactivate annotation edit mode.

**Private annotations** - select what types of annotations are displayed. The types are color-coded:

- **Public annotations** - marked green. Visible for everyone, even unlogged users.
- **My private annotations** - marked purple. Visible for the creater and authorized users, including the administrator
- **Other private annotations** - marked red. Private annotations added by other users of the WebGIS

.. figure:: _static/ann_types_eng_2.png
   :name: ann_types_eng
   :align: center
   :width: 20cm

   Three color-coded types of annotations: 1 - public, 2 - my private, 3 - other private


.. _ngw_annotation_edit:

Web Map: annotation editting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can create and edit annotations, if the option *Edit annotations* on the *"Annotations" panel* is active. When it is active, the mouse pointer has a blue point next to it and a pencil pictogram appears above existing annotations:

.. figure:: _static/ann_edit_option_eng_3.png
   :name: ann_edit_option
   :align: center
   :width: 20cm

   Annotation edit mode (1 - annotation editing enabled, 2 - mouse pointer while creating a line, 3 - edit pictogram appearing when the pointer hovers over the annotation text)

To **create** an annotation you need to click the left mouse button on the Web Map. For a point symbol, only click once. To finish creating a line or a polygon, double click on the last point (polygon will be automatically completed).

Then a dialog window of annotation creation will be opened:

.. figure:: _static/ann_create_en.png
   :name: ann_create
   :align: center
   :width: 20cm

   Dialog window of annotation creation. Point geometry type is selected

Dialog of annotation creation consists of:

- **Editor of annotation message** - WYSIWYG_ editor of the annotation text message.
- **Stroke: width / color** - width and color of the annotation point stroke.
- **Fill color** - color of the annotation point.
- **Circle size, px** - size (diameter) of the annotation point in pixels.

After clicking **Save**, a drop-down menu appears. In it you need to select the type for your annotation - public or private. After you do so, the newly created annotation will appear on the Web Map.

To **edit** annotations you need to activate annotation edit mode, point to an annotation and click the pictogram on it with the left mouse button. The dialog window for annotation editting looks like a dialog window of annotation creation, but has a **"Delete"** button, which allows to delete the chosen annotation. In order to change the font size of the message or its part, you need to select the text first. 
You can edit both your own private annotations and those created by other users if you have the necessary permissions. The type of the annotation is marked at the top of the edit window. For private annotations of other users you will see the creator's name in brackets.

.. _ngw_annotation_perm:

Web Map: user's permissions associated with annotations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To further manage the work with annotations you can use access permissions (you can read more about `setting permissions <https://docs.nextgis.com/docs_ngcom/source/permissions.html#types-of-rules-what-can-be-allowed-or-denied>`_).

There are three permissions associated with annotations:

- **Web Map: View annotations** - allows or forbids annotations viewing by selected users for particular resources. If it is set to Deny value, "Annotations" panel is inactive.
- **Web Map: Edit annotations** - determines whether the selected user can edit public annotations and their own private annotations in a particular resource. If it is set to Deny value, "Edit annotations" option on the "Annotations" panel is inactive.
- **Web Map: Manage annotations** - determines whether the selected user can edit all types of annotation, included those created by other users. If it is set to Deny value, the “Other private annotations” option is not shown on the “Annotations” panel and the other users' private annotations can not be viewed on the Web Map.

Using above-mentioned permissions you can set annotations as following.

In the Settings tab of the Web Map

.. list-table::

   * - Settings
     - Result
   * - | Enable annotations - No
     - | Annotations panel is not shown on the Web Map.
       | Annotations can not be shown on the Web Map.
   * - | Enable annotations - Yes
       | Show annotations by default - No
     - | Annotations panel is available on the Web Map.
       | No tick for "Show annotations layer".
       | Annotations are not displayed on the Web Map when it opens but can be viewed.
   * - | Enable annotations - Yes
       | Show annotations by default - Yes
     - | Annotations panel is available on the Web Map.
       | "Show annotations layer" is ticked.
       | Annotations are displayed on the Web Map when it opens.
       
In the Permissions tab of the Web Map
If annotations are enabled in the Web Map settings, a particular user can have certain permissions:

.. list-table::

   * - Settings
     - Result for **administrator**
     - Result for other users
   * - | Web-map: View annotations - **Deny**
       | annotation_read - Deny
     - | Annotations panel is not shown on the Web Map.
       | No annotations are displayed on the Web Map, even the public annotations that are shown to unlogged users are hidden.
     - | Annotations panel is not shown on the Web Map.
       | No annotations are displayed on the Web Map, even the public annotations that are shown to unlogged users are hidden.
   * - | Web-map: View annotations - **Allow**
       | annotation_read - Allow
     - | Annotations panel is available on the Web Map.
       | Annotations can be viewed.
       | *Editing of annotations is possible*.
     - | Annotations panel is available on the Web Map.
       | Public annotations and *this user's annotations* can be displayed.
       | *Annotations created by other users can not be displayed.*
       | *Annotation editing tools are unavailable*.
   * - | Web-map: View annotations - **Allow**
       | Web-map: Edit annotations - **Deny**
       | annotation_read - Allow
       | annotation_write - Deny
     - | Annotations panel is available on the Web Map.
       | Public annotations and this user's annotations can be displayed.
       | Annotations created by other users can not be displayed.
       | Annotation editing tools are unavailable.
     - | Annotations panel is available on the Web Map.
       | Public annotations and this user's annotations can be displayed.
       | Annotations created by other users can not be displayed.
       | Annotation editing tools are unavailable.
   * - | Web-map: View annotations - **Allow**
       | Web-map: Edit annotations - **Allow**
       | annotation_read - Allow
       | annotation_write - Allow
     - | Annotations panel is available on the Web Map.
       | Public annotations and annotations created by all users can be displayed.
       | Editing of annotations is possible.
     - | Annotations panel is available on the Web Map.
       | Public annotations and *this user's annotations* can be displayed.
       | *Annotations created by other users can not be displayed*.
       | Editing of annotations is possible.
   * - | Web-map: View annotations - **Allow**
       | Web-map: Edit annotations - **Allow**
       | Web-map: Manage annotations - **Allow**
     - | Annotations panel is available on the Web Map.
       | Public annotations and annotations created by all users can be displayed.
       | Editing of annotations is possible.
     - | Annotations panel is available on the Web Map.
       | Public annotations and annotations *created by all users* can be displayed.
       | Editing of annotations is possible.




