.. sectionauthor:: Роман Гайнуллов <roman.gainullov@nextgis.ru>

.. role:: underline
    :class: underline

Data collection on premise
============================

.. _intro:

Introduction
--------

.. note:: 
	**Condition:** This functionality is available for users who have purchased `Extended <https://nextgis.com/pricing/#ngwextended>`_ on premise licence for NextGIS Web.

**Software and components:**

* `NextGIS Formbuilder <https://nextgis.com/nextgis-formbuilder>`_ - desktop app for creating unique forms for field data collection projects.
* `NextGIS Collector <https://nextgis.com/nextgis-collector/>`_ - specialized mobile app for field data collection.
* `NextGIS Web <https://nextgis.com/nextgis-web/>`_ (NGW) - Web GIS deployed on your server to synchronize the data gathered in the field, store it, process, visualize and analyze it.
* `NextGIS ID on-premise <https://docs.nextgis.com/docs_ngid/source/toc.html>`_ (NGIDOP) - user authorization server for managing access to data collection process.

**Roles (users):** Administrator and Data Collectors. **Administrator** sets up and organizes data collection (:underline:`Formbuilder, NGW`). **Data Collectors** only use :underline:`NextGIS Collector` app.

**Data components:** Geodata consists of two components - point coordinates and attributes (description, attachments/photos)


.. _admin_checklist:

Administrator's check-list
-----------------------

1. NGW and NGIDOP are deployed on the server. You have username and password for Administrator.
2. Check NGIDOP settings (configuration provided on demand): sections `OAuth Applications <https://docs.nextgis.com/docs_ngid/source/ngidop.html#oauth-applications>`_ and `NextGIS ID on-premise <https://docs.nextgis.com/docs_ngid/source/ngidop.html#nextgis-id-on-premise-identificator>`_ - make sure collector_hub is set to the address of the server where the GIS is deployed, **otherwise data collection will be impossible**. 
3. Create users for data collectors in the `Team<https://docs.nextgis.com/docs_ngid/source/ngidop.html#ngidop-teams>`_ section of NGIDOP.
4. Open NextGIS Web. In `Control panel <https://docs.nextgis.com/docs_ngcom/source/collector.html#collector-add-members>` create a list of data collectors using names of the Team members added on step 3. Later when the data collection projects are set up, data collectors will be added from that list.
5. Create in NextGIS Web point vector layers that will store collected data. It can be done in the `web interface <https://docs.nextgis.com/docs_ngweb/source/layers.html#ngw-create-vector-layer>`_ or with `NextGIS Formbuilder <https://nextgis.com/nextgis-formbuilder>`_ by creating a data collection form. For more detailed description of form creation see `Formbuilder documentation <https://docs.nextgis.com/docs_formbuilder/source/toc.html>`_. Remember that at this point you'll need to change the authorization `endpoint <https://docs.nextgis.com/docs_formbuilder/source/gui.html#on-premise-authorization>_ from cloud (my.nextgis.com) to the on-premise deployment server.

—--------------------

Now you can set up projects for data collection. NGW has a special type of resource for that, **Collector Project**. You need to enter a name for the resource (it will be visible for data collectors in NextGIS Collector app and for the administrator in the NGW interface). Description and metadata are optional.

6. Set up parameters in the tabs: **Project, Items, Collectors**. In the **Project** tab the administrator enters the username of an NGW user that will be used for adding the collected data to the database. By default it can be the *administrator* user created after NGW is deployed. But we recommend `creating <https://docs.nextgis.com/docs_ngweb/source/admin_tasks.html#ngw-create-user>`_ a special user for the purpose. In the **Items** tab add layers the data will be stored in and basemaps to make the collection process more handy. In the **Collectors** tab select from the list users who will have access to data collection in this project (see step 4). Save the project parameters.

—--------------------

7. Data Collector's work in `NextGIS Collector <https://docs.nextgis.com/docs_collector/source/toc.html>`_. First, as for Formbuilder, data collector must change `authorization server <https://docs.nextgis.com/docs_collector/source/auth.html#via-on-premise-ngidop>`_ to NGIDOP. After that the process is pretty standard for field data collection - log in using credentials given by the administrator (see Team section) and collect data. More on the functionality of the app in the `documentation <https://docs.nextgis.com/docs_collector/source/toc.html>`_.

—--------------------

Additionally, see how the data collection process works `in the cloud on nextgis.com <https://docs.nextgis.com/docs_ngcom/source/collector.html>`_.
