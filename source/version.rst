.. sectionauthor:: Eduard Kazakov, Yulia Grigorenko <yulia.grigorenko@nextgis.com>

.. _versioning:

Versioning
===================

Geodata storage NextGIS Web supports **versioning**, a mechanism that logs changes made to features of vector layers. 

When a feature is created, deleted or edited, this change is registered (who made it and when), and the state of the database before the change is saved in a special way. 

It allows NextGIS Web administrator to:

* Learn who and when made any change to the vector layer;
* Learn who, when and how changed the attributes or the geometry of the feature;
* Access any recorded state of the vector layer and reset incorrect changes;
* Get a list of all the edits made by one user;
* Get a list of all the edits for a given period, for example, a particular date or since a connected database was last updated;

etc. Changes in both **attributes** and **geometry** are logged.

Versioning is **disabled by default**. You can `turn it on <https://docs.nextgis.com/docs_ngweb/source/layers.html#create-vector-layer-vers-pic>`_ for a specific vector layer if needed. Logging activates when the versioning is turned on, the changes made before that are not registered anywhere.

When the versioning is turned off, all the logged information about the changes in the layer is **deleted**.

.. _vers_ngw_ui:

Versioning in the NextGIS Web interface
-----------------------------------------------------

At the moment in the user interface of NextGIS Web you can access two functions:

* Enable and disable versioning in the vector layer settings;
* Additional virtual field "Last changed" in the attribute table of the versioned layer. It allows to see the time and author of the latest change for each feature of the layer.

It is planned to gradually expand the tools for working with versioned layers through the GUI.

.. _vers_ngw_api:

Versioning in NextGIS Web API
---------------------------------------------------------

The main options for obtaining information about versioned layers are now available only in the NextGIS Web API. Through API queries, you can get the states of layers and features for different time periods, the difference between different states, and so on. Here are some examples of API methods for versioned layers:

 - ``/api/resource/{id}`` you can get the information on the versioning status and the current version of the data in the general layer query ('versioning' property).
- ``/api/resource/{id}/feature/`` add layer version to get its state at a particular moment
- ``/api/resource/{id}/feature/changes/check`` to get information about the difference between two given versions of the layer
- ``/api/resource/{id}/feature/version/{vid}`` get metadata for a given version

.. _vers_qgis:

Versioning in QGIS
----------------------------------------------------

Versioning is actively used in QGIS plugin `NextGIS Connect <https://docs.nextgis.com/docs_ngconnect/source/ngconnect.html>`_ that allows integration with NextGIS Web. When versioning is enabled, QGIS can get information about all the changes made to the layer stored in NextGIS Web since the last time it had been accessed. It allows to `simultaneously edit <https://docs.nextgis.com/docs_ngconnect/source/edit.html>`_ a layer stored on the server from several QGIS sessions on separate computers.

