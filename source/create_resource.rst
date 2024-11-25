Добавление ресурсов
===================

NextGIS Web строится на **ресурсном** подходе - каждый компонент системы (слой, группа, сервис) является её ресурсом.
Одним из таких типов ресурсов является **слой** - растровое изображение или векторный файл (таблица базы данных).

Для каждого слоя может быть создано **неограниченное** количество **стилей** - способов визуализации геоданных на веб-карте.

Интерфейс добавления векторных, растровых и PostGIS слоев приблизительно одинаковый - создается ресурс слоя, внутри него - ресурсы стилей.
Последние визуализируют данные на веб-карте.

Для того, чтобы создать ресурс, перейдите в группу, куда хотите его добавить, и нажмите на кнопку **Создать ресурс**. Затем во всплывающем окне выберите нужный тип ресурса. Изначально предлагается полный список доступных ресурсов. 

.. figure:: _static/ngweb_create_ru.png
   :name: ngweb_create_pic
   :align: center
   :width: 20cm

   Окно создания ресурса

Чтобы быстрее найти нужный тип ресурса, можно воспользоваться строкой поиска.

.. figure:: _static/ngweb_create_search_ru.png
   :name: ngweb_create_search_pic
   :align: center
   :width: 20cm

   Поиск нужного типа ресурса

Также типы ресурсов сгруппированы по категориям, которые можно выбрать в левой части окна:

* Слои и `стили <https://docs.nextgis.ru/docs_ngweb/source/mapstyles.html>`_
* Карты и сервисы (`веб-карта <https://docs.nextgis.ru/docs_ngweb/source/webmaps_admin.html>`_, сервисы WMS, WFS, OGC API - Features)
* Сбор данных (`группа трекеров, трекер <https://docs.nextgis.ru/docs_ngcom/source/tracking.html#tracking-create>`_, `проект Collector <https://docs.nextgis.ru/docs_ngcom/source/collector.html#collector-create-project>`_)
* Внешние соединения (соединения PostGIS, TMS, WMS)
* Другие ресурсы (`группа ресурсов <https://docs.nextgis.ru/docs_ngweb/source/layers.html#ngw-resourses-group>`_, `библиотека маркеров SVG <https://docs.nextgis.ru/docs_ngweb/source/layers.html#svg>`_, `справочник <https://docs.nextgis.ru/docs_ngweb/source/layers.html#ngw-create-lookup-table>`_)

Ниже подробно описан процесс создания разных типов ресурсов.
