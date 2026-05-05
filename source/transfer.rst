How to transfer data from one Web GIS to another
================================================

After trying out the functionality in a cloud Web GIS you may decide to deploy an on-premise version of NextGIS Web.

To move your resources to the new Web GIS, you need to download them to your computer and then upload them to the new server.

.. _backup_source:

Create local copy on your device
---------------------------------

The easiest way to do it is via `NextGIS Connect plugin <https://docs.nextgis.com/docs_ngconnect/source/index.html>`_.
`Create a connection <https://docs.nextgis.com/docs_ngconnect/source/ngc_install.html#ng-connect-new-connection>`_ to your old Web GIS. Select resource(s) in the NextGIS Connect panel and click **Add to QGIS**.

If you want to **transfer all the data quickly**, you can select the Main resource group, add it to QGIS and handle the data structure later.

If you want to **preserve the resource structure**, follow these steps:

1. Add **Web Maps** to QGIS, saving each one as separate project. When you download a Web Map in NextGIS Connect, all layers and basemaps added to it are added to the project.
2. Add **resource groups** containing layers that are not added to Web Maps, to one big project. The hierarchic structure of resource groups is preserved.

You can also get the full resource tree of your Web GIS in table format using Toolbox tool `Web GIS structure to spreadsheet <https://toolbox.nextgis.com/t/web_gis_structure>`_.

What resources can be downloaded via NextGIS Connect:

* Vector layers with styles
* Raster layers with styles
* WFS, WMS, TMS layers
* WFS, WMS, OGC API - Features services
* TMS and WMS connections 
* Basemaps
* Lookup tables - they are added to QGIS as value map widget. To upload the lookup table to the new Web GIS you need to export the values to CSV and re-add the lookup table to the layer using value relation widget.

To add multiple resource at once, you can select:

- Web Map - a QGIS project will be created containing layers, styles and basemaps. A mutually exclusive group will be created for all the basemap layers.
* Demo project - a QGIS project will be created containing layers, styles and basemaps;
* Resource group - a new group is created in QGIS containing resources of the types listed above, including Web Maps.

Keep in mind: when you add a Web Map to your QGIS project, all layers are added. So if you select a resource group that contains a Web Map + its layers, there may be duplicates.



.. to_target:

Upload data to the new Web GIS
-----------------------------------

In the NextGIS Connect plugin `create a new connection <https://docs.nextgis.com/docs_ngconnect/source/ngc_install.html#ng-connect-new-connection>`_ to the target Web GIS.

Open QGIS project with the downloaded data.

In the NextGIS Connect panel select a resource group for upload, click **Add to Web GIS** and select **Upload all**. In the pop-up dialog enter the name for the new resource group that is created for the uploaded layers.

When you upload an entire QGIS project, Connect creates a folder that contains all the layers of the project preserving the folder structure, plus one Web Map with all the layers added to it.

If you have created several projects while creating the local copy of your data, you need to repeat the process for each of them.

This way you can transfer most of the resources to the new Web GIS. Some resources, however, are tied to a specific Web GIS, so they need to be created manually.

These are non-transferable resources:

* `Tracker groups  <https://docs.nextgis.com/docs_ngcom/source/tracking.html#tracking-create>`_
* `Trackers <https://docs.nextgis.com/docs_ngcom/source/tracking.html#tracking-create>`_
* `Collector projects <https://docs.nextgis.com/docs_ngweb/source/collector.html#ngw-collector-create-project>`_
* `Tileset <https://docs.nextgis.com/docs_ngweb/source/layers.html#ngw-tile-set>`_
* `File bucket <https://docs.nextgis.com/docs_ngweb/source/create_other.html#ngw-create-file-bucket>`_
* SVG marker library - if you've been using a marker library, we recommend embedding the icons into the style, you can do it with the Toolbox tool `Embed SVG into QGS/QML <https://toolbox.nextgis.com/t/embedsvg>`_



Alternative way to backup data using Toolbox
---------------------------------------------

You can partially transfer resources from one Web GIS to another without using the desktop app with the help of `Toolbox <https://toolbox.nextgis.com/?lang=ru&tags=10>`_.

Create local copy on your device: 

* `Web Map to QGIS project <https://toolbox.nextgis.com/t/webmap2qgis>`_ - saves only the layers added to a Web Map.
* `Vector layers from Web GIS to GeoPackage <https://toolbox.nextgis.com/t/ngw_to_gpkg>`_ - saves only vector layers from the selected resource group.

Upload to Web GIS:

* `Vector layers in Web GIS from archive <https://toolbox.nextgis.com/t/layers2ngw>`_
