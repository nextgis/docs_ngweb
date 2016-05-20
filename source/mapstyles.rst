.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>

.. _ngweb_style_create:
    
Vector layer styles
=====================

Styles describe a way of rendering for geodata and are one of the resources of NGW. Style is added to a map to display geodata.


Creation of style
----------------------------------

Style is related to a single layer so there is no item "Style" in the main resources list. 
To create a style you need to open layer properties of the layer you want create style for. In layer properties page click: 
:menuselection:`Create resource --> MapServer style`. A create resource page for a new style will be opened. You have an option to import a QML style from QGIS or enter the style manually. 

QML style will be converted to internal system format during import. Currently only basic geometry renderer settings are imported.
If a style has a selection by query the empty option should be placed at the end (it is placed first after import from QGIS).

.. warning:: 
   If you created a vector layer but MapServer style is absent in Create resource section  
   check if you have installed nextgis_mapserver package. You can check this using Control panel -> Package versions. 
   If this package is absent it is installed incorrectly. You will need to check executed steps: 
   http://docs.nextgis.ru/docs_ngweb/source/install.html


Map style tags
----------------------------------

To change a style or to create a new one it is recommended you take a code of some existing style and then modify it, so there is no need to start creating a style from scratch.
  
Common tags
~~~~~~~~~~~~~~~~~ 
  
* <color red="255" green="170" blue="127"/> - the color of a fill or a line
* <outlinecolor red="106" green="106" blue="106"/> - outline color
* <width>0.5</width> - a width of a line or an outline of the polygon.
* <outlinewidth>3</outlinewidth> - outline width
* <minscaledenom>1</minscaledenom> - do not display a feature if the map scale is larger than value \
* <maxscaledenom>100000</maxscaledenom> - do not display a feature is the map scale is less than value 

Markers
~~~~~~~~~~~~~~~~~

.. figure:: _static/mapstyle_hatch_demo.png
   :name: ngweb_mapstyle_hatch_demo_pic
   :align: center
   :width: 16cm

   A demo for different hatches.



* <symbol>std:circle</symbol> - marker type

   * std:rectangle - rectangle
   * std:circle - circle
   * std:diamond - diamong
   * std:triangle - triangle with peak at the top
   * std:triangle-equilateral - triangle with peak at the bottom
   * std:star - five-pointed star
   * std:pentagon - pentagon
   * std:arrow - arrow (by default is top oriented. Rotation could be set using a tag <angle>45</angle>)
   * std:cross - +
   * std:xcross - x
   * std:line - short line
   * std:hatch - long line texture

These markers could be used to draw a line, to fill a polygon or to display points. 
Also they may be combined to a complex symbol:

.. code-block:: xml

        <class>
            <expression>"industrial"</expression>
            <!-- Industrial areas -->
            <style> <!-- hatch with a right slope -->
                <color red="255" green="50" blue="50"/>
                <width>1.4</width>
                <symbol>std:hatch</symbol>
                <gap>10</gap>
                <size>5</size>
                <angle>45</angle>
            </style>
            <style> <!-- hatch with a left slope-->
                <color red="255" green="50" blue="50"/>
                <width>1.4</width>
                <symbol>std:hatch</symbol>
                <gap>10</gap>
                <size>5</size>
                <angle>-45</angle>
            </style>
            <style> <!-- Outline -->
                <outlinecolor red="255" green="50" blue="50"/>
                <width>0.5</width>
            </style>
 </class>




* <size>2</size> - marker size in pixels

Line features
~~~~~~~~~~~~~~~~

* <gap>10</gap> - a step size for dashed line (used with <symbol>std:circle</symbol>)
* <width>8</width> - width of line in pixels
* <classitem>PLACE</classitem> - filter by attribute PLACE. Also see example in #Filtering.
  The following operators are supported:
  
  * attribute name
  * !=
  * >=
  * <=
  * <
  * >
  * =* - case insensitive string comparison.

  * =
  * lt - less than
  * gt - greater than
  * ge - greater or equal
  * le - less or equal
  * eq - equal
  * ne - not equal
  * and - AND
  * && - AND
  * or - OR
  * || - OR
  
* <linejoin>round</linejoin> - line draw at corners
* <linecap>round</linecap> - line draw at the beginning and at the end

.. figure:: _static/admin_mapstyles_linecap.png
   :name: admin_mapstyles_linecap.png
   :align: center
   :width: 10cm

   <linecap>butt</linecap> / <linecap>round</linecap> / <linecap>square</linecap>

* <pattern>2.5 4.5</pattern> - dash template 

.. todo:: check for numbers

* <angle> - marker rotation angle. Hatch could also be rotated.

Labels
~~~~~~~~

* <labelitem>a_hsnmbr</labelitem> - attribute name for labelling.
* <minscaledenom>100</minscaledenom> - do not show a label if a scale is larger than 1:1000
* <maxscaledenom>100000</maxscaledenom> - do not show a label if a scale is smaller than1:100000
                
                        

* LABELCACHE [on|off] - specifies whether labels should be drawn as the features for this layer are drawn, or whether they should be cached and drawn after all layers have been drawn. Default is on. Label overlap removal, auto placement etc... are only available when the label cache is active.
* <position>ur</position> - label offset direction.

   * ur - ↗ up and right (recommended).
   * ul - ↖
   * uc - ↑
   * cl - ←
   * cc - centered
   * cr - →
   * ll - ↙
   * lc - ↓
   * lr - ↘
   * auto

* <Maxoverlapangle> - ?  

Some other useful tags
~~~~~~~~~~~~~~~~~~~~~~~

.. todo:: узнать, реализованы ли они

* MAXGEOWIDTH - Maximum width, in the map’s geographic units, at which this LAYER is drawn. If MAXSCALEDENOM is also specified then MAXSCALEDENOM will be used instead.
* MINGEOWIDTH - Minimum width, in the map’s geographic units, at which this LAYER is drawn. If MINSCALEDENOM is also specified then MINSCALEDENOM will be used instead.
* OFFSITE - Sets the color index to treat as transparent for raster layers.
* OPACITY [integer|alpha] - opacity of the layer
* SIZEUNITS [feet|inches|kilometers|meters|miles|nauticalmiles|pixels] - Sets the unit of CLASS object SIZE values (default is pixels). Useful for simulating buffering.
* SYMBOLSCALEDENOM [double] - The scale at which symbols and/or text appear full size. This allows for dynamic scaling of objects based on the scale of the map. If not set then this layer will always appear at the same size. Scaling only takes place within the limits of MINSIZE and MAXSIZE as described above. Scale is given as the denominator of the actual scale fraction, for example for a map at a scale of 1:24,000 use 24000.
* TYPE [chart|circle|line|point|polygon|raster|query] - Specifies how the data should be drawn. Need not be the same as the feature geometry type. For example polygons or polylines may be drawn as a point layer.



Map styles examples
----------------------------------

OSM-default
----------------------------------

Polygon layer with scale range and labels
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

	<map>
	  <layer>
	    <labelitem>a_hsnmbr</labelitem>
	    <class>
	      <style>
		<color red="255" green="170" blue="127"/>
		<outlinecolor red="106" green="106" blue="106"/>
		<width>0.425196850394</width>
		<maxscaledenom>10000</maxscaledenom> <!-- Scale limit -->
	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
		<maxscaledenom>10000</maxscaledenom>
	      </label>
	    </class>
	  </layer>
	</map>


White circle marker
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

     <style>
       <color red="255" green="255" blue="255"/>
       <outlinecolor red="0" green="0" blue="0"/>
       <size>8.50393700787</size>
       <symbol>std:circle</symbol>
     </style>



A line displayed with small black circles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

     <style>
       <angle>auto</angle>
       <gap>-10</gap>
       <color red="255" green="255" blue="255"/>
       <outlinecolor red="0" green="0" blue="0"/>
       <size>2</size>
       <symbol>std:circle</symbol>
     </style>


Filtering
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

	<map>
	  <layer>
	    <labelitem>NAME</labelitem>
	    <classitem>PLACE</classitem>
	    <class>
	      <expression>"city"</expression>
	      <style>
		<color red="255" green="170" blue="0"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>11.3385826772</size>
		<symbol>std:circle</symbol>

	      </style>
	      <style>
		<color red="255" green="170" blue="0"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>5.66929133858</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>18</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		 <position>ur</position>
	      </label>
	    </class>
	    <class>
	      <expression>"town"</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>11.3385826772</size>
		<symbol>std:circle</symbol>

	      </style>
	      <style>
		<color red="0" green="0" blue="0"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>5.66929133858</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>14</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		 <position>ur</position>
	      </label>
	    </class>
	    <class>
	      <expression>"village"</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>6.8031496063</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
	      </label>
	    </class>
	    <class>
	      <expression>"hamlet"</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>4.25196850394</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
	      </label>
	    </class>
	    <class>
	      <expression>"locality"</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>2.83464566929</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>6.5</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
	      </label>
	    </class>
	    <class>
	      <expression>''</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>2.83464566929</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
	      </label>
	    </class>
	  </layer>
	</map>


Polygon layer with a classification by field values and labels
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

	<map>
	<layer>
	  <labelitem>NAME</labelitem>
	    <class>
	      <expression>(([num] gt 18) and ([num] le 26.1))</expression>
	      <style>
		<color red="255" green="255" blue="212"/>
		<outlinecolor blue="64" green="64" red="64"/>

	      </style>
	       <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
		<maxscaledenom>7000000</maxscaledenom>
	      </label>
	    </class>
	  
	      <class>
	      <expression>(([num] gt 26.1) and ([num] le 28.1))</expression>
	      <style>
	       <color red="254" green="217" blue="142"/>
		<outlinecolor blue="64" green="64" red="64"/>

	      </style>
		 <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
		<maxscaledenom>7000000</maxscaledenom>
	      </label>
	    </class>
	  
	  
	    <class>
	      <expression>(([num] gt 28.1) and ([num] le 30))</expression>
	      <style>
	       <color red="254" green="153" blue="41"/>
		<outlinecolor blue="64" green="64" red="64"/>

	      </style>
	       <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
		<maxscaledenom>7000000</maxscaledenom>
	      </label>
	    </class>
	  
	  </layer>
	</map>




OSM settlement-point
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

	<!-- Style with different settings for different scales-->
	<!-- Версия 2015-07-24 -->
	<map>
	  <layer>
	    <labelitem>NAME</labelitem>
	    <classitem>PLACE</classitem>
	    <class>
	      <expression>"city"</expression> <!-- Большой город -->
	      <style>
		<color red="255" green="170" blue="0"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>11.3385826772</size>
		<symbol>std:circle</symbol>

	      </style>
	      <style>
		<color red="255" green="170" blue="0"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>5.66929133858</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>18</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		 <position>ur</position>
	      </label>
	    </class>
	    <class>
	      <expression>"town"</expression> <!-- Small or medium city -->
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>11.3385826772</size>
		<symbol>std:circle</symbol>
		<maxscaledenom>6000000</maxscaledenom>

	      </style>
	      <style>
		<color red="0" green="0" blue="0"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>5.66929133858</size>
		<symbol>std:circle</symbol>
		<maxscaledenom>6000000</maxscaledenom>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>14</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		 <position>ur</position>
		<maxscaledenom>6000000</maxscaledenom>
	      </label>
	    </class>
	    <class>
	      <expression>"village"</expression> <!-- Village  -->
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>6.8031496063</size>
		<symbol>std:circle</symbol>
		<maxscaledenom>1000000</maxscaledenom>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
		<maxscaledenom>1000000</maxscaledenom>
	      </label>
	    </class>
	    <class>
	      <expression>"hamlet"</expression> <!-- Hamlet -->
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>4.25196850394</size>
		<symbol>std:circle</symbol>
		<maxscaledenom>500000</maxscaledenom>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
		<maxscaledenom>500000</maxscaledenom>
	      </label>
	    </class>
	    <class>
	      <expression>"locality"</expression> <!-- Non inhabited place -->
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>2.83464566929</size>
		<symbol>std:circle</symbol>
		<maxscaledenom>500000</maxscaledenom>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>6.5</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
		<maxscaledenom>500000</maxscaledenom>
	      </label>
	    </class>
	    <class>
	      <expression>''</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>2.83464566929</size>
		<symbol>std:circle</symbol>

	      </style>
	      <label>
		<type>truetype</type>
		<font>regular</font>
		<size>8.25</size>
		<color blue="0" green="0" red="0"/>
		<outlinewidth>3</outlinewidth>
		<outlinecolor blue="255" green="255" red="255"/>
		<position>ur</position>
	      </label>
	    </class>
	  </layer>
	</map>


OSM highway-lowzoom
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Public roads (small roads are in a separate style). Colorscheme from openstreetmap.de

.. figure:: _static/mastyles_osm-highway-lowzoom.png
   :name: mastyles_osm-highway-lowzoom
   :align: center

   Fragment of colorscheme for public roads. 

.. code-block:: xml


    <map>
    <!-- Highways for low-zoom from openstreetmap (from motorway to residential) version 2015-11-06 -->
        <layer>
            <classitem>Highway</classitem>
            <labelitem>Name</labelitem>
            <class>
                <expression>"motorway"</expression>
                <style>
                    <color red="185" green="49" blue="49" />
                    <linejoin>round</linejoin>
                    <width>8</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="226" green="114" blue="114" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="255" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                </label>
            </class>
            <class>
                <expression>"motorway_link"</expression>
                <style>
                    <color red="185" green="49" blue="49" />
                    <linejoin>round</linejoin>
                    <width>8</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="226" green="114" blue="114" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="255" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
            </class>
            <class>
                <expression>"trunk"</expression>
                <style>
                    <color red="185" green="49" blue="49" />
                    <linejoin>round</linejoin>
                    <width>8</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="226" green="114" blue="114" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="255" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                </label>
            </class>
            <class>
                <expression>"trunk_link"</expression>
                <style>
                    <color red="185" green="49" blue="49" />
                    <linejoin>round</linejoin>
                    <width>8</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="226" green="114" blue="114" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="255" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
            </class>
            <class>
                <expression>"primary"</expression>
                <style>
                    <color red="141" green="67" blue="70" />
                    <linejoin>round</linejoin>
                    <width>6.4062992126</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="226" green="114" blue="114" />
                    <linejoin>round</linejoin>
                    <width>3.57165354331</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                </label>
            </class>
            <class>
                <expression>"primary_link"</expression>
                <style>
                    <color red="141" green="67" blue="70" />
                    <linejoin>round</linejoin>
                    <width>6.4062992126</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="226" green="114" blue="114" />
                    <linejoin>round</linejoin>
                    <width>3.57165354331</width>
                    <linecap>round</linecap>
                </style>
            </class>
            <class>
                <expression>"secondary"</expression>
                <style>
                    <color red="163" green="123" blue="72" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="246" green="232" blue="86" />
                    <linejoin>round</linejoin>
                    <width>3</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                </label>
            </class>
            <class>
                <expression>"secondary_link"</expression>
                <style>
                    <color red="163" green="123" blue="72" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="246" green="232" blue="86" />
                    <linejoin>round</linejoin>
                    <width>3</width>
                    <linecap>round</linecap>
                </style>
            </class>
            <class>
                <expression>"tertiary"</expression>
                <style>
                    <color red="187" green="187" blue="187" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="179" />
                    <linejoin>round</linejoin>
                    <width>3</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                </label>
            </class>
            <class>
                <expression>"tertiary_link"</expression>
                <style>
                    <color red="187" green="187" blue="187" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="179" />
                    <linejoin>round</linejoin>
                    <width>3</width>
                    <linecap>round</linecap>
                </style>
            </class>
            <class>
                <expression>"unclassified"</expression>
                <style>
                    <color red="187" green="187" blue="187" />
                    <linejoin>round</linejoin>
                    <width>4</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="179" />
                    <linejoin>round</linejoin>
                    <width>3</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                    <minscaledenom>1</minscaledenom>
		            <maxscaledenom>40000</maxscaledenom> 
                </label>
            </class>
            <class>
                <expression>"residential"</expression>
                <style>
                    <color red="187" green="187" blue="187" />
                    <linejoin>round</linejoin>
                    <width>2</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="179" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                    <minscaledenom>1</minscaledenom>
		            <maxscaledenom>40000</maxscaledenom> 
                </label>
            </class>
            <class>
                <expression>"living_street"</expression>
                <style>
                    <color red="187" green="187" blue="187" />
                    <linejoin>round</linejoin>
                    <width>2</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="179" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                    <minscaledenom>1</minscaledenom>
		            <maxscaledenom>40000</maxscaledenom> 
                </label>
            </class>
        </layer>
    </map>


OSM highway-maxzoom
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Access roads, service roads, dirt roads, pedestrian ways


.. figure:: _static/mastyles_osm-highway-highzoom.png
   :name: mastyles_osm-highway-highzoom
   :align: center
   :width: 10cm

   Fragment of road map.

.. code-block:: xml

    <map>
     <!-- Highways for high-zoom from openstreetmap (from service to track) version 2015-11-06 -->
        <layer>
            <classitem>Highway</classitem>
            <labelitem>Name</labelitem>
            <class>
                <expression>"service"</expression>
                <style>
                    <color red="187" green="187" blue="187" />
                    <linejoin>round</linejoin>
                    <width>2</width>
                    <linecap>round</linecap>
                </style>
                <style>
                    <color red="255" green="255" blue="255" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
            </class>
            <class>
                <expression>"footway"</expression>
                <style>
                    <color red="255" green="0" blue="0" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                </style>
                <label>
                    <type>truetype</type>
                    <font>regular</font>
                    <size>7</size>
                    <color blue="0" green="0" red="0" />
                    <outlinewidth>1</outlinewidth>
                    <outlinecolor blue="255" green="255" red="255" />
                    <angle>follow</angle>
                    <antialias>true</antialias>
                    <repeatdistance>300</repeatdistance>
                    <maxoverlapangle>20.0</maxoverlapangle>
                </label>
            </class>
            <class>
                <expression>"pedestrian"</expression>
                <style>
                    <color red="255" green="0" blue="0" />
                    <linejoin>round</linejoin>
                    <width>2</width>
                    <linecap>round</linecap>
                </style>
            </class>
            <class>
                <expression>"path"</expression>
                <style>
                    <color red="255" green="0" blue="0" />
                    <linejoin>round</linejoin>
                    <width>1</width>
                    <linecap>round</linecap>
                    <pattern>5 5</pattern>
                </style>
            </class>
            <class>
                <expression>"track"</expression>
                <style>
                    <color red="153" green="116" blue="43" />
                    <linejoin>round</linejoin>
                    <width>2</width>
                    <pattern>16 8</pattern>
                    <linecap>round</linecap>
                </style>
            </class>
        </layer>
    </map>

railway-line
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

	<!-- railway-line style with different display for different scales 
	version 2015-07-24 -->
	<map>
	  <layer>
	    <classitem>RAILWAY</classitem>
	    <class>
	      <expression>"abandoned"</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<linejoin>round</linejoin>
		<width>2.83464566929</width>
		<linecap>round</linecap>
	      </style>
	      <style>
		<pattern>2.35275590551 4.70551181102</pattern>
		<color red="165" green="165" blue="165"/>
		<linejoin>round</linejoin>
		<width>2.35275590551</width>
		<linecap>round</linecap>   
	      </style>
	    </class>
		<class>
	      <expression>"razed"</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<linejoin>round</linejoin>
		<width>2.83464566929</width>
		<linecap>round</linecap>
	      </style>
	      <style>
		<pattern>2.35275590551 4.70551181102</pattern>
		<color red="255" green="165" blue="210"/>
		<linejoin>round</linejoin>
		<width>2.35275590551</width>
		<linecap>round</linecap>   
	      </style>
	    </class>
	    <class>
	      <expression>"construction"</expression>
	      <style>
		<color red="255" green="255" blue="255"/>
		<linejoin>round</linejoin>
		<width>2.83464566929</width>
		<linecap>round</linecap>     
	      </style>
	      <style>
		<pattern>2.35275590551 4.70551181102</pattern>
		<color red="255" green="0" blue="127"/>
		<linejoin>round</linejoin>
		<width>2.35275590551</width>
		<linecap>round</linecap>    
	      </style>
	    </class>
	    <class>
	      <expression>"crossing"</expression>
	      <style>
		<color red="37" green="37" blue="255"/>
		<linejoin>bevel</linejoin>
		<width>0.737007874016</width>
		<linecap>square</linecap>
	      </style>
	    </class>
	    <class>
	      <expression>"light_rail"</expression>
	      <style>
		<color red="0" green="0" blue="0"/>
		<linejoin>bevel</linejoin>
		<width>1.41732283465</width>
		<linecap>square</linecap>
	      </style>
	    </class>
	    <class>
	      <expression>"narrow_gauge"</expression>
	      <style>
		<color red="150" green="150" blue="150"/>
		<linejoin>bevel</linejoin>
		<width>1.41732283465</width>
		<linecap>square</linecap> 
	      </style>
	    </class>
	    <class>
	      <expression>"platform"</expression>
	      <style>
		<color red="0" green="0" blue="0"/>
		<linejoin>bevel</linejoin>
		<width>4.25196850394</width>
		<linecap>square</linecap>   
	      </style>
	    </class>
	    <class>
	      <expression>"rail"</expression>
	      <style>
		<color red="0" green="0" blue="0"/>
		<linejoin>bevel</linejoin>
		<width>2.83464566929</width>
		<linecap>square</linecap> 
		<maxscaledenom>25000</maxscaledenom> <!-- Black and white line at large scale -->
	      </style>
	      <style>
		<pattern>9.41102362205 14.1165354331</pattern>
		<color red="255" green="255" blue="255"/>
		<linejoin>bevel</linejoin>
		<width>2.35275590551</width>
		<linecap>square</linecap>
		<maxscaledenom>25000</maxscaledenom> <!-- Black and white line at large scale -->
	      </style>
	       <style>
		
		<color red="0" green="0" blue="0"/>
		<linejoin>bevel</linejoin>
		<width>2</width>
		<linecap>square</linecap>
		<minscaledenom>25000</minscaledenom> <!-- Black line at medium scale -->
	      </style>
	    </class>
	    <class>
	      <expression>"siding"</expression>
	      <style>
		<color red="145" green="145" blue="145"/>
		<linejoin>bevel</linejoin>
		<width>1.41732283465</width>
		<linecap>square</linecap>  
	      </style>
	    </class>
	    <class>
	      <expression>"subway"</expression>
	      <style>
		<pattern>1.41732283465 2.83464566929</pattern>
		<color red="155" green="155" blue="155"/>
		<linejoin>round</linejoin>
		<width>1.41732283465</width>
		<linecap>round</linecap>
	      </style>
	    </class>
	    <class>
	      <expression>"tram"</expression>
	      <style>
		<color red="0" green="0" blue="0"/>
		<linejoin>bevel</linejoin>
		<width>1.41732283465</width>
		<linecap>square</linecap>
	      </style>
	    </class>
	  </layer>
	</map>


OSM water-line
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

	<!-- water-line style with different display for different scales-->
	<!-- Версия 2015-07-24 -->
	<map>
	  <layer>
	    <classitem>Waterway</classitem>
	    <labelitem>name</labelitem>
	    <class>
	      <expression>"river"</expression>
	      <style>
		<color red="102" green="153" blue="204"/>
		<linejoin>round</linejoin>
		<width>3</width>
		<linecap>round</linecap>
		<!-- Unprocessed attributes: width_unit, offset_unit, customdash_unit -->
	      </style>
	      <label>
		<type>truetype</type> <!-- Подпись -->
		<font>bold</font>
		<size>7</size>
		<color blue="255" green="255" red="255"/>
		<outlinewidth>1</outlinewidth>
		<outlinecolor red="102" green="153" blue="204"/>
		<angle>auto</angle>
		<repeatdistance>300</repeatdistance>
		<maxoverlapangle>90.0</maxoverlapangle>
		<maxscaledenom>500000</maxscaledenom>
	      </label>
	      </class> 
	    
	      <class>
	      <expression>"canal"</expression>  
	      <style><!-- вертикальные линии -->
		<angle>auto</angle>
		<gap>-8.50393700787</gap>
		<!-- Остались необработанные атрибуты: interval_unit, placement, offset_unit, offset -->
		<color red="102" green="153" blue="204"/>
		<outlinecolor red="0" green="0" blue="0"/>
		<size>15.66929133858</size>
		<symbol>std:line</symbol>
		<!-- Unprocessed attributes: outline_width, offset_unit, outline_width_unit, size_unit -->
	      </style>
	      <style>
		<color red="102" green="153" blue="204"/>
		<linejoin>round</linejoin>
		<width>3</width>
		<linecap>round</linecap>
		<!-- Unprocessed attributes: width_unit, offset_unit, customdash_unit -->
	      </style>
	      <label>
		<type>truetype</type> <!-- Подпись -->
		<font>bold</font>
		<size>7</size>
		<color blue="255" green="255" red="255"/>
		<outlinewidth>1</outlinewidth>
		<outlinecolor red="102" green="153" blue="204"/>
		<angle>auto</angle>
		<repeatdistance>300</repeatdistance>
		<maxoverlapangle>90.0</maxoverlapangle>
		<maxscaledenom>500000</maxscaledenom>
	      </label>
	      </class> 
	    
	      <class>
	      <expression>"stream"</expression>
	      <style>
		<color red="102" green="153" blue="204"/>
		<linejoin>round</linejoin>
		<width>1.5</width>
		<linecap>round</linecap>
		<maxscaledenom>250000</maxscaledenom>
		<!-- Unprocessed attributes: width_unit, offset_unit, customdash_unit -->
	      </style>
	      </class> 
	    
	      <class>
	      <expression>"drain"</expression>
	      <style>
		<color red="102" green="153" blue="204"/>
		<linejoin>round</linejoin>
		<width>1</width>
		<linecap>round</linecap>
		<maxscaledenom>250000</maxscaledenom>
		<!-- Unprocessed attributes: width_unit, offset_unit, customdash_unit -->
	      </style>
	      </class> 
	  </layer>
	</map>

OSM water-polygon
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: xml

	<!--  water-polygon style
	version 2015-07-24 
	To add 
	-reservoirs
	-swamp hatch
	-->
	<map>
	  <layer>
	    <labelitem>NAME</labelitem>
	    <classitem>NATURAL</classitem>
	    <class>
	      <expression>"water"</expression> <!-- Water -->
	      <style>
		<color red="102" green="153" blue="204"/>
		<outlinecolor red="102" green="153" blue="204"/>
	      </style>
		 <label>
		<type>truetype</type>
		<font>regular</font>
		<size>7</size>
		<color red="102" green="153" blue="204"/>
		<outlinewidth>2</outlinewidth>
		<outlinecolor red="255" green="255" blue="222"/>
		<!-- Label scale range-->
		<minscaledenom>1</minscaledenom>
		<maxscaledenom>100000</maxscaledenom>    
	      </label>
	    </class>
	    <class>
	      <expression>"wetland"</expression> <!-- Wetland -->
		  <style>
		<color red="102" green="153" blue="204"/>
		<outlinecolor red="102" green="153" blue="204"/>
	      </style>
		 <label>
		<type>truetype</type>
		<font>regular</font>
		<size>7</size>
		<color red="102" green="153" blue="204"/>
		<outlinewidth>2</outlinewidth>
		<outlinecolor red="255" green="255" blue="222"/>
		<!-- Label scale range -->
		<minscaledenom>1</minscaledenom>
		<maxscaledenom>100000</maxscaledenom>    
	      </label>
	    </class>
	  </layer>
	</map>




OSM-black
----------------------------------

OSM landuse-polygon
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NextGIS Web styles support for different hatched (see  :numref:`ngweb_mapstyle_hatch_demo_pic`).

.. code-block:: xml


	<map> <!-- A demo of different hatched. Use with dark background.-->
	    <layer>
		<labelitem>OSM_ID</labelitem>
		<classitem>LANDUSE</classitem>
		<class>
		    <expression>"residential"</expression>
		    <!-- Residential -->
		    <style>
		        <!-- hatch with right slope -->
		        <color red="255" green="185" blue="33"/>
		        <width>1.4</width>
		        <symbol>std:line</symbol>
		        <gap>3</gap>
		        <size>1</size>
		        <angle>90</angle>
		    </style>
		    <style>
		        <!-- Outline -->
		        <outlinecolor red="255" green="185" blue="33"/>
		        <width>0.5</width>
		    </style>
		</class>
		<class>
		    <expression>"grass"</expression>
		    <!-- Grass zones -->
		    <style>
		        <!-- Lines -->
		        <color red="20" green="255" blue="33"/>
		        <width>1</width>
		        <symbol>std:line</symbol>
		        <gap>6</gap>
		        <size>4</size>
		        <angle>0</angle>
		        <pattern>2.5 4.5</pattern>
		    </style>
		    <style>
		        <!-- Outline -->
		        <outlinecolor red="20" green="255" blue="33"/>
		        <width>0.5</width>
		    </style>
		</class>
		<class>
		    <expression>"commercial"</expression>
		    <!-- Residential -->
		    <style>
		        <!-- hatch with right slope -->
		        <color red="133" green="33" blue="25"/>
		        <width>1.4</width>
		        <symbol>std:line</symbol>
		        <gap>10</gap>
		        <size>5</size>
		        <angle>45</angle>
		    </style>
		    <style>
		        <!-- Outline -->
		        <outlinecolor red="133" green="33" blue="25"/>
		        <width>0.5</width>
		    </style>
		</class>
		<class>
		    <expression>"industrial"</expression>
		    <!-- Industrial zones -->
		    <style>
		        <!-- hatch with right slope -->
		        <color red="255" green="50" blue="50"/>
		        <width>0.4</width>
		        <symbol>std:hatch</symbol>
		        <gap>10</gap>
		        <size>5</size>
		        <angle>45</angle>
		    </style>
		    <style>
		        <!-- hatch with left slope-->
		        <color red="255" green="50" blue="50"/>
		        <width>0.4</width>
		        <symbol>std:hatch</symbol>
		        <gap>10</gap>
		        <size>5</size>
		        <angle>-45</angle>
		    </style>
		    <style>
		        <!-- Outline -->
		        <outlinecolor red="255" green="50" blue="50"/>
		        <width>0.5</width>
		    </style>
		</class>
		<class>
		    <expression>"cemetery"</expression>
		    <!-- Cemeteries -->
		    <style>
		        <!-- fences -->
		        <color red="14" green="166" blue="0"/>
		        <width>1.4</width>
		        <symbol>std:rectangle</symbol>
		        <gap>20</gap>
		        <size>11</size>
		        <angle>0</angle>
		    </style>
		    <style>
		        <!-- fences -->
		        <color red="0" green="0" blue="0"/>
		        <width>1.2</width>
		        <symbol>std:rectangle</symbol>
		        <gap>20</gap>
		        <size>10</size>
		        <angle>0</angle>
		    </style>
		    <style>
		        <!-- crosses -->
		        <color red="14" green="166" blue="0"/>
		        <width>1.4</width>
		        <symbol>std:cross</symbol>
		        <gap>20</gap>
		        <size>9</size>
		        <angle>0</angle>
		    </style>
		    <style>
		        <!-- Outline -->
		        <outlinecolor red="14" green="166" blue="0"/>
		        <width>0.5</width>
		    </style>
		</class>
	    </layer>
	</map>
