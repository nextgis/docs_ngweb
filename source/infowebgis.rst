
.. _ngw_storage:

Storage
--------

The "Storage" section contains information about the volume of data loaded into Web GIS depending on their type.
The space usage estimate is located below the main table.
The administrator can forcibly recalculate the amount of storage (for example - immediately after loading big data, if the system has not yet recalculated the occupied space on its own).

.. figure:: _static/admin_storage_panel_settings_eng.png
   :name: admin_storage_panel_settings
   :align: center
   :width: 18cm

   Storage section



.. _ngw_backups:

Backups
-------

In this section you can see a list of available NextGIS Web backups, as well as download any of them.
The process of creating backups and restoring for developers is described in `this section <https://docs.nextgis.ru/docs_ngweb_dev/doc/admin/backup_restore.html>`_.



.. _ngw_system_info:

System information
------------------

Through the control panel, the administrator can view information about the system and the current version of the platform (see :numref:`admin_system_info_rus_eng`). Using the icon in the upper right corner, you can copy all this data to the clipboard.

.. figure:: _static/admin_system_info_eng_2.png
   :name: admin_system_info_rus_eng
   :align: center
   :width: 16cm

   System information section in the control panel


.. figure:: _static/admin_system_info1_rus_eng.png
   :name: admin_system_info1_rus_eng
   :align: center
   :width: 16cm

   System and platform information


.. _ngw_audit:

User activity log
------------------

User requests to the Web GIS are logged in a journal. It can be found in the **Info** section of the Control panel of the Web GIS (:numref:`control_panel_audit_pic`).

.. figure:: _static/control_panel_audit_en.png
   :name: control_panel_audit_pic
   :align: center
   :width: 9cm
   
   Request journal in the Web GIS Control panel

The log is presented in a form of a table that has a set of filters above it (:numref:`user_activity_log_pic`). Every user action is registered in the journal. The entry contains the following details:

* Timestamp
* Request (includes `response status codes <https://developer.mozilla.org/en-US/docs/Web/HTTP/Status>`_ and `request method <https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods>`_ )
* IP address
* User
* Route name
* Context (type and ID of the resource)

  
.. figure:: _static/user_activity_log_en.png
   :name: user_activity_log_pic
   :align: center
   :width: 24cm
   
   Activity journal

You can filter the journal entries by time period and user performing the action (:numref:`audit_filter_pic`). The table, filtered or otherwise, can be exported as a CSV file.

.. figure:: _static/audit_filter_en.png
   :name: audit_filter_pic
   :align: center
   :width: 20cm
   
   Filtering by timestamp and user

To view the complete text of the request click on the corresponding entry (:numref:`audit_log_entry_pic`).

.. figure:: _static/audit_log_entry_en.png
   :name: audit_log_entry_pic
   :align: center
   :width: 20cm
   
   Log entry



