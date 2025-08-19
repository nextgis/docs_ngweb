.. _ngw_general:

Общие сведения
==============

Программное обеспечение NextGIS Web представляет собой картографическое
веб-приложение. Серверная Веб :abbr:`ГИС (геоинформационная система)` с открытым исходным кодом, предназначенная для
поддержки ввода, хранения, визуализации и предоставления регулируемого доступа к
результатам картографической или космической деятельности, интегрированных в единый
банк данных и метаданных.

Программное обеспечение NextGIS Web разработано на базе программного обеспечения
с открытым исходным кодом (:term:`Open Source`). Программное обеспечение NextGIS
Web поддерживает протоколы открытого обмена данными Open Geospatial Consorcium
(`OGC <http://www.opengeospatial.org/>`_) и отвечает современным требованиям к
архитектуре приложений реализуемой на базе свободного программного обеспечения
(СПО).

NextGIS Web позволяет:

1. `Создавать <https://docs.nextgis.ru/docs_ngweb/source/webmaps_admin.html>`_ и `отображать <https://docs.nextgis.ru/docs_ngweb/source/webmaps_client.html>`_ карты.
2. Выполнять `навигацию <https://docs.nextgis.ru/docs_ngweb/source/webmaps_client.html#ngw-webmaps-client-scale>`_ по карте (увеличение, уменьшение, перемещение).
3. Управлять наполнением карты через веб-интерфейс.
4. Подключать `векторные <https://docs.nextgis.ru/docs_ngweb/source/layers.html#ngw-create-vector-layer>`_ (:term:`ESRI Shapefile`, :term:`GeoJSON`, :term:`PostGIS` и др. ) и `растровые <https://docs.nextgis.ru/docs_ngweb/source/layers.html#ngw-create-raster-layer>`_ данные (:term:`GeoTIFF`).
5. Использовать стандартные протоколы (:term:`WMS`, :term:`WFS-T`, :term:`TMS`, OGC API Features), чтобы `подключать загруженные данные в сторонние приложения <https://docs.nextgis.ru/docs_ngweb/source/services.html>`_ и `добавлять данные из внешних источников <https://docs.nextgis.ru/docs_ngweb/source/connections.html>`_.
6. Гибко `настраивать права доступа <https://docs.nextgis.ru/docs_ngcom/source/permissions.html>`_ к слоям, группам слоёв, картам и ресурсам других видов.
7. Взаимодействовать `посредством API <https://docs.nextgis.ru/docs_ngweb_dev/doc/developer/toc.html>`_.

.. raw:: html

   <iframe width="720" height="405" src="https://rutube.ru/play/embed/9323ef11a606d852933f080c48e9ed1d/" frameBorder="0" allow="clipboard-write; autoplay" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe>

Посмотреть видео на `youtube <https://youtu.be/GzReGU2oACI>`_, `rutube <https://rutube.ru/video/9323ef11a606d852933f080c48e9ed1d/>`_.

NextGIS Web состоит из серверной и клиентской части.

Серверная часть занимается хранением и отрисовкой геоданных. Она написана на
языке программирования Python с использованием фреймворка Pyramid. Клиентская
часть предоставляет пользовательский интерфейс для интерактивного управления
:term:`геоданными <геоданные>` и взаимодействия с ними через карту.

Клиент разработан на языке JavaScript. Вся конфигурация системы хранится 
внутри базы данных PostgreSQL, так же используется модуль расширения
:term:`PostGIS`. Разметка страниц интерфейса пользователя проводится с
использованием языка разметки HTML, оформление страниц интерфейса пользователя
- с использованием таблиц описания стилей – CSS. Описание запросов к
базам данным выполнено на языке SQL.

NextGIS Web – модульная система, состоящая из ряда обязательных и вспомогательных
модулей. Вспомогательные модули могут быть включены или отключены на этапе
конфигурации. Компоненты NextGIS Web взаимодействуют между собой
посредством вызовов внутренних методов API.

NextGIS Web функционирует под управлением операционной системы семейства
Linux (рекомендуется использовать дистрибутивы на базе Debian, например Ubuntu
Server). Подробнее см. подраздел :ref:`ngw_soft_req`. NextGIS Web работает во всех современных браузерах.

Внешний вид пользовательского интерфейса с опубликованной веб-картой представлен
на :numref:`webmap_sample`.

.. figure:: _static/webmap_sample2_rus_2.png
   :name: webmap_sample
   :align: center
   :width: 20cm

   Пользовательский интерфейс с опубликованной веб-картой

.. _ngw_keyfeatures:

Основные возможности NextGIS Web
--------------------------------

NextGIS Web обладает следующими основными возможностями:

.. _ngw_keyfeatures_layers:

Слои данных
~~~~~~~~~~~

* Создание `растровых <https://docs.nextgis.ru/docs_ngweb/source/layers.html#ngw-create-raster-layer>`_ и `векторных <https://docs.nextgis.ru/docs_ngweb/source/layers.html#ngw-create-vector-layer>`_ слоев и загрузка данных для них через веб-интерфейс.
* Создание и подключение `WMS <https://docs.nextgis.ru/docs_ngweb/source/connections.html#ngw-wms-in>`_ и `TMS <https://docs.nextgis.ru/docs_ngweb/source/connections.html#ngw-tms-in>`_ слоев.
* Создание и подключение `PostGIS <https://docs.nextgis.ru/docs_ngweb/source/postgis_details.html>`_ слоев из внешних баз данных.
* Выбор из базовых подложек: OpenStreetMap, Google, Bing и иных, доступных в каталоге `QuickMapServices <https://qms.nextgis.com/>`_.
* Сервис `WFS <https://docs.nextgis.ru/docs_ngweb/source/services.html#ngw-wfs-service>`_.
* Сервис `WMS <https://docs.nextgis.ru/docs_ngweb/source/services.html#ngw-wms-service>`__.
* Сервис `OGC API Features <https://docs.nextgis.ru/docs_ngweb/source/services.html#ngw-OGC-API-Features>`_.
* `Справочник <https://docs.nextgis.ru/docs_ngweb/source/create_other.html#ngw-create-lookup-table>`_ (при наличии модуля расширения).
* `Набор файлов <https://docs.nextgis.ru/docs_ngweb/source/create_other.html#ngw-create-file-bucket>`_ (при наличии модуля расширения).
* Поддержка `метаданных <https://docs.nextgis.ru/docs_ngweb/source/edit_resource.html#ngw-update-info-metada>`_ (в виде "ключ-значение").
* Экспорт в различные форматы (см. `список доступных форматов <https://docs.nextgis.ru/docs_ngcom/source/data_export.html#ngcom-data-export>`_).

.. _ngw_keyfeatures_access:

Управление доступом
~~~~~~~~~~~~~~~~~~~

* Детальная `настройка прав доступа <https://docs.nextgis.ru/docs_ngcom/source/permissions.html>`_ для всех ресурсов и групп ресурсов.
* Глобальные права доступа к Веб ГИС `для пользователей и групп пользователей <https://docs.nextgis.ru/docs_ngweb/source/users.html#ngw-group-rights>`_.

.. _ngw_keyfeatures_render:

Отрисовка и символика
~~~~~~~~~~~~~~~~~~~~~

* Импорт символики QGIS с автоматической конвертацией "под рендерер".
* Подключаемые рендереры: :term:`MapServer`, :term:`QGIS` (импорт
  проекта из ПО NextGIS QGIS c сохранением состава слоев, стилей и др.).
* Несколько вариантов символики для одного слоя данных.

.. _ngw_keyfeatures_webmap:

Веб-карты
~~~~~~~~~

* Неограниченное количество веб-карт.
* Свой набор слоев и `управление деревом слоев <https://docs.nextgis.ru/docs_ngweb/source/webmaps_admin.html#ngw-map-layers>`_ для каждой карты.
* Повторное использование одного и того же представления слоя в разных картах.

.. _ngw_keyfeatures_ui:

Интерфейс пользователя
~~~~~~~~~~~~~~~~~~~~~~

* `Дерево слоев <https://docs.nextgis.ru/docs_ngweb/source/admin_interface.html#ngw-view-resource>`_.
* `Группы слоев <https://docs.nextgis.ru/docs_ngweb/source/create_resource.html#ngw-resource-group>`_.
* Панель `инструментов навигации <https://docs.nextgis.ru/docs_ngweb/source/webmaps_client.html#ngw-webmaps-client-tools>`_.
* `Поиск по атрибутам <https://docs.nextgis.ru/docs_ngweb/source/webmaps_client.html#ngw-webmaps-client-search>`_.
* Закладки для быстрого перехода на нужные участки карты.
* Просмотр описания слоя.
* Просмотр таблицы атрибутов объектов слоя с быстрым переходом от таблицы к карте.
* `Аннотации <https://docs.nextgis.ru/docs_ngweb/source/annotation.html>`_.

.. _ngw_keyfeatures_edit:

Редактирование
~~~~~~~~~~~~~~

* Редактирование `значений атрибутов объектов <https://docs.nextgis.ru/docs_ngweb/source/layers_settings.html#ngw-attributes>`_.
* Редактирование `описания слоя <https://docs.nextgis.ru/docs_ngweb/source/edit_resource.html#ngw-update-info-metada>`_.
* Прикрепление `фотографий и других вложений <https://docs.nextgis.ru/docs_ngweb/source/layers_settings.html#ngw-attachments>`_.
* Редактирование по протоколу WFS-T.

.. _ngw_sys_req:

Минимальное аппаратное обеспечение
------------------------------------

Минимальные параметры системы для работы :abbr:`ПО (программное
обеспечение)` NextGIS Web:

* 4 процессорных ядра,
* оперативная память не менее 8 GiB,
* 250 GB HDD (лучше SSD).

.. _ngw_soft_req:

Рекомендуемые версии базового ПО
---------------------------------

В качестве браузеров рекомендуется использовать:

* Google Chrome (версия 118 и выше)
* Mozilla Firefox (версия 115 и выше)
* Edge (версия 116 и выше)
* Safari (версия 17 и выше)
* Opera (версия 104 и выше)

Браузер Microsoft Internet Explorer не поддерживается.

.. warning::
   Работа с другими версиями основного ПО возможна, но не гарантирована.

Технические требования для установки на своём сервере см. `здесь <https://docs.nextgis.ru/docs_ngweb_dev/doc/admin/install.html#system-requirements>`_.
