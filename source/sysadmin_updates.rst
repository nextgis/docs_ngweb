.. sectionauthor:: Maxim Dubinin <maxim.dubinin@nextgis.com>

.. _sysadmin_tasks:
    
Latest changes
==============

This section describes latest changes to the software.

4.3.0 released 21.11.2022
~~~~~~~~~~~~~~~~~~~~~~~~~
* Support for webmap cloning via UI.
* Search by coordinates on web maps.
* CSV and XLSX support when creating a vector layer.
* Export and import feature layer attachments.
* Vector layer export to KML and KMZ formats.
* Fields selection while exporting feature layer.
* Assign default groups while creating users via UI.
* Experimental support for authorization links.
* Use resource SRS by default while exporting raster and vector layers.
* Support for booleans and nulls in resource metadata.
* Support for fixed length character columns in PostGIS layers.
* Support for materialized views and 25D geometries in PostGIS layers.
* Ability to turn off user password and keep only OAuth authentication.
* Check for disk free space in the healthcheck.
* Ability to search through resources recursively in REST API.
* OpenLayers library upgraded to 6.15.1.

4.2.0 released 18.07.22
~~~~~~~~~~~~~~~~~~~~~~~
* "Locate me" tool on web maps.
* Identifiable setting for web map layers.
* Batch deletion and moving of resources.
* Ability to download raster layers as an internal representation.
* PostGIS connection and layer diagnostic tool.
* Support for quad-key basemaps on web maps.
* OAuth improvements: NextGIS ID integration, simultaneous authorization code and password grant types.
* Improved management of spatial reference systems and catalog integration.
* Better support for 25D geometries on web maps and PostGIS layers.
* Improved handling of URLs in descriptions and feature layer fields.
* Cloud-optimized GeoTIFF (COG) enabled by default.
* In-place conversion between COG and non-COG rasters.
* Hide empty groups and groups with no accessible layers on web maps.
* M dimension stripping while creating vector layers in LOSSY mode.
* Selecting features on web maps via hl_* URL parameters.
* Ability to inject some HTML into the base template for metrics and counters.
* Fast JSON serialization and deserialization based on orjson library.
* Completed control panel migration to Antd and React.

4.1.0 released 16.02.22
~~~~~~~~~~~~~~~~~~~~~~~
* Cloud-optimized GeoTIFF (COG) support for raster layers.
* Browser compatibility test and Internet Explorer deprecation.
* Experimental support for long-runnning requests for raster and vector layers creation using lunkwill extension.
* Private annotations on web maps, visible only for authors.
* Wrapping around the dateline for tile-based layers on web maps.
* A lot of improvements for the control panel: filters, batch operations, etc.
* Improved handling of vector layer sources with id and geom fields.
* Reprojection into different coordinate systems in WMS and WFS services.
* Export feature layer using field display names (aliases) instead of keynames.
* Support for CORS domain wildcards (like https://*.csb.app).
* WFS client and server simple filters support.
* Improved handling of coordinates outside boundaries of coordinate systems.
* Support for 25D geometries in PostGIS layers.
* Ability to filter NULL values in feature REST API.
* Unknown fields in REST API filters return an error.
* Improved handling of external services errors and timeouts.
* Upgraded dependencies: Pyramid 2.0, SQLAlchemy 1.4, and OpenLayers 6.10

4.0.0 released 18.11.21
~~~~~~~~~~~~~~~~~~~~~~~
* Source layer selection while creating vector layers from multi-layer sources, such as ZIP-archives or Mapinfo TABs.
* On-the-fly reprojection for WMS and WFS services.
* Ability to restrict address search by a country if using Nominatim.
* Hide inaccessible layers while displaying web maps.
* Highlight feature when selecting from search results.
* Display emails as active mailto: links in the webmap popup.
* Ability to delete users and groups from the control panel.
* Ability to change resource owner in UI and REST API.
* Automatic generation of keynames for WMS and WFS services.
* Improved support for Unicode field names for WFS services.
* Granular control setting for resource export availability.
* ISO-8601 date and time formatting in feature layer REST API via dt_format=iso option.
* Drop Python 2.7 support, NextGIS Web now requires Python 3.8+.
* PostgreSQL 10+, PostGIS 2.5+ and GDAL 3.0+ are required now.
* Synchronization of translations with POEditor.
* Yandex Maps-based address search on the webmap.

3.9.0 released 11.08.21
~~~~~~~~~~~~~~~~~~~~~~~
* Simple tool for previewing resources on the map.
* Resource quick search tool in the page header.
* Disable/enable address search via settings in the control panel.
* Ability to constraint address search area by web map initial extent.
* Zoom to a better extent from address search and bookmark panel.
* Language autodetection, per-user language setting, and support for the external translation files.
* Automatic downsampling of a social preview image to 1600x630 pixels.
* Better support for KML: LIBKML GDAL driver is used when available.
* Filtering features by ID in feature REST API.
* Layers with an "id" field can be loaded if the field has an integer type.
* Information about available distribution versions in the control panel.
* Experimental storage accounting and estimation subsystem.

3.8.0 released 12.05.21
~~~~~~~~~~~~~~~~~~~~~~~
* Ability to constraint a web map to the default extent.
* More length and area units in web map settings.
* Automatic correction of errors during the creation of a vector layer.
* Support for creation of vector layers from GML and KML files.
* User login is case insensitive when logging in.
* Configuration option for disabling social networks sharing buttons.
* Performance improvements in geometry handling and rendering, especially when converting between WKT and WKB formats.
* Performance improvements in tile cache component.
* Improved word wrapping in web map identification popup.
* Minimum and maximum scale restrictions in WMS server.
* Experimental integration of modern JavaScript and Webpack.
* Quota for the maximum number of enabled users.
* OpenLayers library upgraded to 6.5.0.
* OAuth server logout support via logout redirect endpoint.

3.7.0
~~~~~
* Add database migrations framework and automatic migrations applying.
* External access links for styles, web maps (TMS), and feature layers (MVT).
* Experimental WFS client and raster mosaic, which is disabled by default.
* Add support of 1.1.0 version in WFS server implementation.
* Improved handling of NODATA values in raster layer and raster style.
* Compression level of PNG images is set to 3, which is much faster.
* Performance improvements and better concurrency for tile cache.
* New "CSV for Microsoft Excel" export format for better Excel compatibility.
* Fix infinite wait of database lock, including during vector layer deletion.
* Improved handling of invalid JSON bodies in RESP API, now the correct error message is returned.
* Vector layer export to MapInfo MIF/MID format.
* Vector layer export to Panorama SXF format.

3.6.0
~~~~~
* Major improvements and bug fixes in WFS protocol implementation.
* Permission model changes: now any action on resource requires read permission from scope resource on the resource and its parent.
* PostGIS layer extent calculation and improved extent calculation in vector layer.
* Vector layer export to GeoPackage format.
* Faster processing of empty tiles and images.
* Tile cache and webmap annotations are enabled by default.
* Command to delete orphaned vector layer tables.
* HTTP API with resource permissions explanation.
* Support for like, geom and extensions in feature layer REST API.
* Support for GeoJSON files in ZIP-archive and faster ZIP-archive unpacking.
* Clickable resource links in webmap, WMS and WFS services.
* Ability to disable SSL certificate check for TMS connection.
* Lookup table component is part of nextgisweb core package nextgisweb.
* Fix TMS layer tile composition in case of extent outside the bounds.
* Fix GDAL > 3 compability issues, including axis orientation.
* SVG marker library resource available to renderers.

3.5.0
~~~~~
* Raster layer export to GeoTIFF, ERDAS IMAGINE and Panorama RMF formats.
* Customizable link preview for resources.
* Improved resource picker: inappropriate resources are disabled now.
* New implementation of WFS server which fixes many bugs.
* Quad-key support in TMS connection and layer.
* Support for geom_format and srs in feature layer REST API (POST / PUT requests).
* Session-based OAuth authentication with token refresh support.
* Delete users and groups via REST API.
* Track timestamps of user's last activity.
* Customization of web map identify popup via control panel.
* Speedup cleanup of file storage maintenance and cleanup.
* Fix bulk feature deletion API when passing an empty list.
* Fix bug in CORS implementation for requests returning errors.
* Fix coordinates display format in web map identification popup.
* Fix tile distortion issue for raster styles

3.4.2
~~~~~
* Fix WMS layer creation.

3.4.1
~~~~~
* Fix layout scroll bug in vector layer fields editing.

3.4.0
~~~~~
* New tus-based file uploader. Check for size limits before starting an upload.
* Server-side TMS-client. New resource types: TMS connection and TMS layer.
* Create, delete and reorder fields for existing vector layer.
* Improved Sentry integration.
* WMS service layer ordering.
* Stay on the same page after login.
* Error messages improvements on trying to: render non-existing layer, access non-existing attachment or write a geometry to a layer with a different geometry type.

2020-06-30 release
~~~~~~~~~~~~~~~~~~
* General. Add/remove fields of attributes table.
* General. Reorder fields of attributes table.

2020-06-24 release
~~~~~~~~~~~~~~~~~~
* General. Support raster pyramids for QGIS style for raster layers.

2020-06-05 release
~~~~~~~~~~~~~~~~~~
* General. New data uploader. Check for size limits before starting an upload.
* General. Stay on the same page on login to the same page.
* General. Human readable error on trying to access non-existing attachment.
* General. Human readable error on trying to render non-existing layer.
* General. Human readable error on trying to write a geometry to a layer with a different geometry type.
* General. Improve handling rasters with huge size.
* Extensions. Whitelabel - new extension to set corporate interface elements (logos, links, company mentions etc.).

2020-04-16 release
~~~~~~~~~~~~~~~~~~
* For developers. Single feature extent endpoint. Example: https://demo.nextgis.com/api/resource/1735/feature/1/extent
* For developers. Ordering for data filtering. Reverse ordering and two and more field ordering are supported. Example: https://demo.nextgis.com/api/resource/1731/feature/?limit=10&order_by=NAME,-LEISURE
* Admin GUI. Prohibit blocking of the last (the only) administrator in the system.

2020-03-03 release
~~~~~~~~~~~~~~~~~~~
* Services. Fix declared CRS for WMS containing raster layers.
* Services. Fix RGBA conversion to JPG on WMS requests.

2020-02-12 release
~~~~~~~~~~~~~~~~~~~
* Storage. Support for storing Z-type geometries, PolygonZ etc.
* For developers. API can accept and provide Z-type geometries.

2019-11-18 release
~~~~~~~~~~~~~~~~~~~
* Storage. Support for numeric-type fields on layers added from external PostgreSQL/PostGIS
* Search. Improve address search (uses Nominatim)
* For developers. In addition to style IDs Web Map API now provides layer IDs.

2019-11-06 release
~~~~~~~~~~~~~~~~~~~
* Printing. Zooming with the box now correctly fit the zoomed area with chosen paper format (A4 etc.)

2019-10-17 release
~~~~~~~~~~~~~~~~~~

* CRS. Import from ESRI WKT (in addition to OGC WKT)
* CRS. Unicode in CRS names is now supported.
* CRS. Identification doesn't crash anymore if CRS transformation was not possible.

2019-08-12 release
~~~~~~~~~~~~~~~~~~

* Web Map. Search for integer values in added to the embedded feature table.
* Web Map. Improved zooming on a point from the embedded feature table.
* Web Map. While editing the embedded feature table is correctly updated to show newly added features.

