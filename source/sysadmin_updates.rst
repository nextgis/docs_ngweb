

.. _sysadmin_tasks:
    
Changelog
==========

This section describes the latest changes to the software.

**3.2.0 released on 21 Apr 2026**

* The Gallery resource: a new way to organize resources for end users.
* Support for feature filtering on webmaps and in the rendering HTTP API.
* S3-based storage for external raster layers with read/write support.
* Field value suggestions in feature layer filters.
* RAT attribute support for raster layer identification.
* Feature versioning enabled by default for new vector layers.
* Improved resource table and actions UI.
* Support for labels in user-defined SLD styles.
* Support for upscaling tileset layers.
* Automatic GeoTIFF compression predictor selection.
* JPEG compression enabled by default for RGB(A) raster layer pyramids.
* GPX export support for feature layers.
* Improved export performance for layers with many fields.
* Support for the Referer header in WMS and TMS client connections.
* Option to disable TLS certificate verification for WMS connections.
* Support for remote SRS in WMS client layers.
* Built-in SRS catalog based on the Proj database.
* Support for custom OnlineResource values in WMS client layers.
* TypeScript upgraded to 6.0.
* Ant Design upgraded to 6.2.


**3.1.0 released on 21 Jan 2026**


* Advanced feature table filtering capabilities.
* Viewing feature versioning history in the UI.
* Reverting versioned feature layers to previous states.
* Ability to create vector layers from existing layers and versions.
* Improved extent selector widget in webmap settings.
* Display total and filtered feature counts in feature tables.
* Dimmed out-of-scale-range layers in the tree on webmaps.
* Improvements to the printable webmap legend.
* Display measurement spatial reference system on webmaps.
* Transactional HTTP API for feature attachments and descriptions.
* Fixed PostGIS layer editing on webmaps.
* SQLAlchemy library upgraded to 2.0.


**3.0.0 released on 14 Oct 2025**


* Many new feature editing tools on webmaps.
* Improved swipe tool: vertical and horizontal swipes in any direction.
* Maximum webmap zoom level increased to 24.
* Geometry editing on feature pages.
* Geometry tab on feature pages.
* Zoom support for attachment preview.
* Automatic HEIF-to-JPEG conversion for feature attachments.
* Improved handling of image dimensions in descriptions.
* Improved WMS client compatibility and performance.
* Improved MVT generation performance.
* GeoPackage 1.4 compatibility improvements.
* Dropped support for GeoJSON response streaming.
* Dropped support for GeoJSON range requests.
* React library upgraded to 19.


**2.9.0 released on 23 Jun 2025**


* Identification of raster layers on webmaps.
* Improved webmap links, preserving legend symbols in URLs.
* Ability to configure panels and tools individually for each webmap.
* Refined resource picker behavior for single and multi-selection.
* Configurable lookup table item order, including manual ordering.
* Experimental adaptive layouts for webmaps on mobile and tablet devices.
* Initial experimental support for UI form designer.
* Handle rendering artifacts at tile boundaries for QGIS styles.
* Fix `@map_scale` variable and area calculations for QGIS styles.
* Support for dash patterns in user-defined styles.
* Support for adding fields to layers with versioning enabled.
* Creation of raster layers from ZIP archives and PAM support. 
* Support for GDAL PAM georeferenced (.aux.xml) JPEG and PNG rasters.
* Configurable default basemaps for on-premises installations.
* Improved handling of date and bigint fields in UI and HTTP API.
* Improved handling of `geom` fields in feature layers.
* Clean up unreferenced file objects during maintenance by default.
* Python 3.10+ is now the minimum required version.
* OpenLayers library upgraded to 10.4.
* Support for adding specific Formbuilder forms to Collector projects.
* Support for using PostGIS layers as data sources in Collector projects.
* Export stops as waypoints in GPX files.

**2.8.2 released on 22 mar 2025**

* Fix NextGIS QGIS authentication.

**2.8.1 released on 6 Jun 2025**

* Fix layer symbols toggling.

**2.8.0 released on 05 Mar 2025**


* Webmap migrated to React with many new features.
* The identification panel enabled by default, replacing webmap popups.
* Resource editing interface fully migrated to React.
* New feature display page and modal dialog with vector tile geometries.
* Enhanced resource deletion confirmation with details of resources to delete.
* On-demand webmap legend symbol loading, optimizing webmap opening time.
* Configurable SSL mode for PostGIS connections.
* Handling of invalid geometries for MVT vector tiles.
* Better support for date and time fields in WFS clients.
* Expanded JSON output format capabilities in WMS services.
* Improved handling of `fid` fields in feature layers.
* Extended resource search HTTP API.
* Pre-calculated and cached style scale ranges for faster webmap opening.
* Chrome 118+, Safari 17+, Edge 116+, or Firefox 115+ is required.
* All legacy AMD modules migrated to TypeScript, and Dojo dependency removed.
* Experimental support for Python 3.10
* Improved track display with a distinguishable style.
* Fixed rounding of speed, direction, and HDOP in webmap popup.


**2.7.0 released on 06 October 2024**


* New resource creation dialog.
* Identification panel replaces popup on the webmap.
* Font management in the control panel.
* Preview layers with leaving the current page.
* Preview of the basemap on the resource editing page.
* Support for empty geometries when importing vector layers.
* Customizable web map title.
* WFS client component turned on by default.
* Data structure permission scope removed.
* TUS uploader is available using CORS.
* All of resource widgets migrated to Antd and React.
* Fix uploading of zero-length files.


**2.6.1 released on 20 Sep 2024**


* Fix audit journal display.
* Fix WMS service layer adding.


**2.6.0 released on 17 Jul 2024**

* Support for raster style legends.
* Customizable print layout including legend.
* Exclusive layer groups on webmaps.
* User-defined favorite resource pages.
* Measurement SRS configuration per-webmap.
* Experimental support for feature versioning.
* Better handling of incomplete geometry parts.
* Show feature attachment descriptions in the viewer.
* Extent information in PostGIS layer diagnostics.
* Transparent annotation polygons for better visibility.
* Switch to nearest neighbor resampling for raster pyramids.
* Experimental support for NULL geometries.
* Support for upper-case placeholders in TMS connections.
* Metadata permission scope and webmap display permission removed.
* Sentry client-side JavaScript integrations.
* Cache seeding and invalidation removed, use NextGIS Geoservices instead.
* Most of resource widgets migrated to Antd and React.

**2.5.2 released on 26 Jun 2024**

* Authorization fixes.

**2.5.1 released on 26 Arp 2024**

* Fix rendering of legend categories disabled by default.


**2.5.0 released on 04 Apr 2024**


* Turn legend categories on/off for a better experience on webmaps.
* Lots of customization options for embedded webmaps: map tools and panels.
* Ability to exclude feature layer fields from text search.
* Configurable permissions for groups, users, SRS and CORS management.
* Enhanced feature attachment viewer now includes panoramic images support.
* Ctrl+click opens feature attachment in a fixed tab.
* Ability to copy printing parameters as a URL for sharing.
* Support for custom SVG logos in addition to PNG.
* Add export to CSV and import from CSV for lookup tables.
* Support for multiple replicas and long-runnning requests.
* Update minimum required NodeJS version to 20.0.
* Fix permission dependencies on QGIS style parent resources.
* Copying QGIS styles from one to another.

**2.4.3 released on 02 Mar 2024**

* Expanded list of allowed CORS headers.

**2.4.2 released on 2 Feb 2024**

* Fix styles for WFS layers.

**2.4.1 released on 29 Dec 2023**

* Fix NextGIS ID configuration issue.
* Support for PostgreSQL 12.


**2.4.0 released on 28 Dec 2023**


* Support for lookup tables for feature layer attributes.
* Print to TIFF format in addition to PDF, JPEG and PNG.
* Geometry-based filtering of feature tables on webmaps.
* Autodetection of minimum and maximum scales for webmap and WMS layers.
* Refresh feature tables after saving layer changes on webmaps.
* Improved handling of ``id`` and ``fid`` attributes of vector layers.
* Improved handling of date and time inputs.
* Passing a zoom level to webmaps via ``zoom`` attribute.
* Support for OpenID Connect UserInfo endpoint.
* Extraction of scale ranges from QML styles.

**2.3.1 released on 17 Oct 2023**

* Fix layers visibility when printing a web map.
* Fix SLD marker preview size.
* Fix IP addresses adding to CORS settings.



**2.3.0 released on 28 Sep 2023**

* New tileset resource for storing and serving prerendered tiles.
* New OGC API Features service with read and write support.
* Up to 2x speed-up of loading vector layer data.
* Differentiate webmap intial and contstraining extents.
* Support for Google Analytics metrics.
* Support for basic user-defined styles.
* Save to PDF from the webmap printing panel.
* Check effective permissions of other users for a resource.
* Legends for webmaps is enabled by default.
* Lots of improvements in feature editing widgets.
* OutputFormat declaration in WFS for better compatibility.
* Store audit journal in PostgreSQL database instead of ElasticSearch.
* React library upgraded to 18.
* Ant Design library upgraded to 5.

**2.2.0 released on 30 Jun 2023**

* Auto-generated and configurable legends for webmaps.
* Reordering layers via drag-and-drop while viewing webmaps.
* Changing layer opacity while viewing webmaps.
* Creation of an empty vector layer without uploading a file.
* Ability to replace existing vector layer features and fields from a file.
* Brand-new feature table based on React.
* Resource and feature description editors updated to CKEditor 5.
* Improved handling of resource descriptions on webmaps.
* Zoom to a filtered set of features on webmaps.
* Geometry properties in the identification popup.
* Show the cursor location and the current extent on webmaps.
* Zoom to all layers on webmaps.
* Support for linear and polygonal annotations.
* Default display names for resources during creation.
* Deletion of all features and changing geometry type for vector layers.
* Limit by extent while exporting feature layers.
* Ability to export a filtered set of features.
* MapInfo formats support when creating a vector layer.
* TMS client: parallel fetching of tiles and HTTP/2.
* Reasonable resource tabs ordering and auto-activation.
* Improved usability of the layers tree on webmaps.
* Fast PNG compression for rendering.
* Chrome 102+, Safari 15+, Edge 109+ or Firefox 102+ is required.
* User permissions section is moved to a separate page.
* OAuth-based automatic group assignment.

**2.1.3 released on 17 Mar 2023**

* Fix Collector issues.


**2.1.2 released on 1 Jul 2023**

* Fix feature attachment download issues.


**2.1.1 released on 23 Nov 2022**

* Fix resource group selection issue while cloning webmaps


**2.1.0 released on 21 Nov 2022**

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
* Support for fixed length ``character`` columns in PostGIS layers.
* Support for materialized views and 25D geometries in PostGIS layers.
* Ability to turn off user password and keep only OAuth authentication.
* Check for disk free space in the healthcheck.
* Ability to search through resources recursively in REST API.
* OpenLayers library upgraded to 6.15.1.

**2.0.0 released on 18 Jul 2022**

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
* Selecting features on web maps via ``hl_*`` URL parameters.
* Ability to inject some HTML into the base template for metrics and counters.
* Fast JSON serialization and deserialization based on orjson library.
* Completed control panel migration to Antd and React.

**1.9.0 released on 16 Feb 2022**

* Cloud-optimized GeoTIFF (COG) support for raster layers.
* Browser compatibility test and Internet Explorer deprecation.
* Experimental support for long-runnning requests for raster and vector layers creation using ``lunkwill`` extension.
* Private annotations on web maps, visible only for authors.
* Wrapping around the dateline for tile-based layers on web maps.
* A lot of improvements for the control panel: filters, batch operations, etc.
* Improved handling of vector layer sources with ``id`` and ``geom`` fields.
* Reprojection into different coordinate systems in WMS and WFS services.
* Export feature layer using field display names (aliases) instead of keynames.
* Support for CORS domain wildcards (like ``https://*.csb.app``).
* WFS client and server simple filters support.
* Improved handling of coordinates outside boundaries of coordinate systems.
* Support for 25D geometries in PostGIS layers.
* Ability to filter NULL values in feature REST API.
* Unknown fields in REST API filters return an error.
* Improved handling of external services errors and timeouts.
* Upgraded dependencies: Pyramid 2.0, SQLAlchemy 1.4, and OpenLayers 6.10.

**1.8.0 released on 18 Nov 2021**

* Source layer selection while creating vector layers from multi-layer sources, such as ZIP-archives or Mapinfo TABs.
* On-the-fly reprojection for WMS and WFS services.
* Ability to restrict address search by a country if using Nominatim.
* Hide inaccessible layers while displaying webmaps.
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

**1.7.3 released on 8 Oct 2021**

* Fix vector layer loading with error feature skip.
* Fix exception handling of tile cache writing.

**1.7.2 released on 1 Oct 2021**

* Use SameSite=None for the session cookie to support iframe authentication.

**1.7.1 released on 25 Aug 2021**

* Fix MapServer Python 3 compability issue.
* Fix critical Webpack compability error.

**1.7.0 released on 11 Aug 2021**

* Simple tool for previewing resources on the map.
* Resource quick search tool in the page header.
* Disable/enable address search via settings in the control panel.
* Ability to constraint address search area by web map initial extent.
* Zoom to a better extent from address search and bookmark panel.
* Language autodetection, per-user language setting, and support for the external translation files.
* Automatic downsampling of a social preview image to 1600x630 pixels.
* Better support for KML: LIBKML GDAL driver is used when available.
* Filtering features by ID in feature REST API.
* Layers with an "id" field can be loaded if the field has an int32 type.
* Information about available distribution versions in the control panel.
* Experimental storage accounting and estimation subsystem.

**1.6.0 released on 12 May 2021**

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

**1.5.0 released on 14 Feb 2021**

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

