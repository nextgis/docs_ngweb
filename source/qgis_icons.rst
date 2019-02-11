.. sectionauthor:: Denis Rykov <denis.rykov@nextgis.ru>

.. _ngw_qgis_icons:
    
Using additional SVG symbols
============================

You can use SVG symbols for your QGIS styles. NextGIS QGIS has a default set of icons, but you can also use your own. 

Internal set is located here: C:/NextGIS/share/ngqgis/svg

Example: http://trolleway.nextgis.com/resource/2061/display?panel=layers

Using your own markers
----------------------

If you need a marker which is not part of standard library, you can use a hyperlink for this marker while creating a style in QGIS. You can upload this style and the marker will also be functional. You'll need to host your marker somewhere to be available via hyperlink.

Example: https://demo.nextgis.com/resource/4177/display?panel=layers

SVG symbols search paths
------------------------

If your QGIS style uses local SVG symbols and server renderer can't find them - they won't rendered. 
To help renderer locate them you need to expand search paths. To do that update
configuration file ``config.ini`` and add a list of additional search paths to ``qgis`` section:

.. code-block:: bash

    [qgis]

    svgpaths = path1, path2, ..., pathn

If your symbols are located in subfolders and style file contains something like this:

.. code-block:: bash

    <prop v="dir1/dir2/dir3/icon.svg" k="name"/>

your path in ``svgpaths`` should also include these subfolders, where ``dir1`` can be found.
