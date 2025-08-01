Векторные тайлы MVT
===================

При `создании векторного слоя <https://docs.nextgis.ru/docs_ngweb/source/layers.html#ngw-create-vector-layer>`_ автоматически генерируется ссылка, с помощью которой загруженные данные можно подключить в формате :term:`MVT` (Map Vector Tiles, `спецификация <https://github.com/mapbox/vector-tile-spec>`_).

.. _mvt_advantage:

Преимущества формата MVT
-------------------------

Представление MVT позволяет хранить и передавать большие объёмы географических данных в компактном виде. Это делает его особенно полезным для веб-карт и мобильных приложений, где использование сетевых ресурсов и быстрый доступ к данным являются ключевыми.

Главные преимущества формата:

* Скорость отрисовки данных;
* Динамичные подписи и элементы карты;
* Широкие и гибкие возможности по настройке стилей данных (фильтры отображения по масштабу, временны́м или иным данным).

Каждый тайл является самодостаточным и содержит все необходимые данные для отображения конкретного уровня масштабирования (zoom level) определённого прямоугольного региона на карте. Данные организованы в слои, которые могут содержать разные типы геометрии и атрибуты.

MVT поддерживается различными библиотеками и инструментами для чтения, записи и отображения тайлов: 

* библиотеки Mapbox GL, OpenLayers, Leaflet, 
* инструменты для создания и обработки MVT-тайлов, такие как Tippecanoe и Mapbox Studio.

.. _mvt_link:

Формирование ссылки MVT
------------------------

При создании векторного слоя в NextGIS Web автоматически генерируется ссылка типа:

.. raw:: html

    <code><strong>https://demo.nextgis.ru/</strong>api/component/feature_layer/mvt?resource=<strong>6503</strong>&amp;z={z}&amp;x={x}&amp;y={y}</code>

Жирным выделены URL-адрес Веб ГИС и номер ресурса (векторного слоя). 

Такую ссылку можно использовать в сторонних веб-приложениях и конструкторах карт. Ссылка передаёт данные о слое, **стиль** для слоя настраивается на стороне клиента. 

Ссылка расположена на странице ресурса в разделе "Внешний доступ".

.. figure:: _static/mvt_link_ru.png
   :name: mvt_link_pic
   :align: center
   :width: 20cm

   Ссылка на векторные тайлы MVT на странице ресурса

.. _mvt_ngfrontend:

Подключение с помощью библиотеки NextGIS Frontend
---------------------------------------------------

Достаточно знать только URL-адрес вашей Веб ГИС и номер ресурса, чтобы подключить векторный слой в виде MVT в ваше приложение. Для этого используются разработанные нашей командой JS-библиотеки.

Все библиотеки доступны в открытом сервисе `NextGIS Frontend <https://code.nextgis.com/>`_.

Там же можно найти примеры использования библиотек. Возьмём `один из таких примеров <https://code.nextgis.com/ngw-mapbox-examples-ngw-mvt-match-paint>`_, чтобы отобразить наш векторный слой.

URL Веб ГИС и номер ресурса указываются в соответствующих строках.

.. figure:: _static/mvt_frontend_code_ru.png
   :name: mvt_frontend_code_pic
   :align: center
   :width: 20cm



При открытии HTML-страницы мы увидим карту. Настроить дополнительный функционал можно также с помощью NextGIS Frontend.

.. figure:: _static/mvt_frontend_map_ru.png
   :name: mvt_frontend_map_pic
   :align: center
   :width: 20cm


Чтобы данные из вашей Веб ГИС отображались во внешних приложениях, надо `открыть доступ к слою на чтение <https://docs.nextgis.ru/docs_ngcom/source/permissions.html>`_ в настройках в Веб ГИС. Либо можно указать данные для входа в Веб ГИС в само веб-приложение.

В настройках Веб ГИС нужно задать свои `настройки CORS <https://docs.nextgis.ru/docs_ngweb/source/cors.html>`_.

.. _mvt_qgis:

Подключение MVT из NextGIS Web в QGIS
--------------------------------------

Предоставляемую NextGIS Web ссылку на векторные тайлы можно добавить в QGIS. Таким образом вы сможете использовать представление векторного слоя из вашей Веб ГИС в настольном приложении.

Чтобы подключить тайлы, выберить в меню добавления слоя в QGIS «Добавить векторный тайловый слой».

.. figure:: _static/mvt_qgis_ru.jpg
   :name: mvt_qgis_pic
   :align: center
   :width: 20cm

Такой слой можно использовать как подложку, для него можно настраивать стиль. 

Работает идентификация объектов с описанием атрибутов.


.. _ngw_connect_tms_gdal:

Использование сервиса TMS
==========================

NextGIS Web является сервером TMS. Соответственно подключить созданные в нем слои/стили можно в любом клиентском ПО, поддерживающем протокол TMS. Для этого нужно знать URL сервиса TMS.

Эта ссылка располагается на странице ресурса **стиля** в разделе "Внешний доступ":

.. figure:: _static/tms_link_style_ru.jpg
   :name: tms_link_style_pic
   :align: center
   :width: 20cm

Ссылка формируется следующим образом, пример:

.. raw:: html

    <code><strong>https://demo.nextgis.ru/</strong>api/component/render/tile?z={z}&x={x}&y={y}&resource=<strong>234</strong></code>

Для использования TMS через утилиты GDAL нужно создать для него файл XML и подставить эту ссылку в строку ``ServerUrl`` примера ниже. Всё остальное остаётся неизменным.

.. code-block:: xml

   <GDAL_WMS>
    <Service name="TMS">
        <ServerUrl>https://demo.nextgis.ru/api/component/render/tile?z={z}&x={x}&y={y}&resource=234</ServerUrl>
    </Service>
    <DataWindow>
        <UpperLeftX>-20037508.34</UpperLeftX>
        <UpperLeftY>20037508.34</UpperLeftY>
        <LowerRightX>20037508.34</LowerRightX>
        <LowerRightY>-20037508.34</LowerRightY>
        <TileLevel>18</TileLevel>
        <TileCountX>1</TileCountX>
        <TileCountY>1</TileCountY>
        <YOrigin>top</YOrigin>
    </DataWindow>
    <Projection>EPSG:3857</Projection>
    <BlockSizeX>256</BlockSizeX>
    <BlockSizeY>256</BlockSizeY>
    <BandsCount>4</BandsCount>
    <Cache />
   </GDAL_WMS> 
