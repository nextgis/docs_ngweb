.. _ngcom_webmap_optimize:

How to optimize web map performance
=======================================


**Tile cache**

Set up `tile caching <https://docs.nextgis.com/docs_ngweb/source/mapstyles.html#tile-cache>`_ for your layer styles.
In this case, already rendered data will not be re-requested.


**Zoom scale**

Set a range of `scale levels <https://docs.nextgis.com/docs_ngweb/source/webmaps_admin.html#admin-webmap-create-layers>`_ for displaying data on a web map.
This will allow you not to request data outside this range.
