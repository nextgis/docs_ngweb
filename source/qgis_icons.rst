.. sectionauthor:: Denis Rykov <denis.rykov@nextgis.ru>

.. _ngw_qgis_icons:
    
User SVG symbols
================

SVG symbols search paths
------------------------

If your QGIS style uses SVG symbols and server renderer can't find them - they won't rendered. 
To help renderer locate them you need to expand search paths. To do that update
configuration file ``config.ini`` and add a list of additional search paths to ``qgis`` section:

.. code-block:: bash

    [qgis]

    svgpaths = path1, path2, ..., pathn

If your symbols are located in subfolders and style file contains something like this:

.. code-block:: bash

    <prop v="dir1/dir2/dir3/icon.svg" k="name"/>

your path in ``svgpaths`` should also include these subfolders, where ``dir1`` can be found.
