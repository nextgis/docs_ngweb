.. sectionauthor:: Артём Светлов <artem.svetlov@nextgis.ru>
.. sectionauthor:: Роман Гайнуллов <roman.gainullov@nextgis.ru>

.. |button_delete| image:: _static/button_delete.png
   :width: 6mm
   :alt: мусорная корзина

.. |button_edit| image:: _static/button_edit.png
   :width: 6mm
   :alt: карандаш

.. |button_open_web_map| image:: _static/button_open_web_map.png
   :width: 6mm
   :alt: карта с лупой

.. |button_open_feature_table| image:: _static/button_open_feature_table.png
   :width: 6mm
   :alt: табличка

.. |button_preview| image:: _static/button_preview.png
   :width: 6mm
   :alt: глаз в рамке

.. _ngw_admin_interface:

Основные элементы интерфейса
================================

.. _ngw_admin_login:

Авторизация
-----------

Для того, чтобы авторизоваться, откройте Веб ГИС и нажмите **Войти** в правом верхнем углу.

.. figure:: _static/ngweb_before_signin_ru.png
   :name: ngweb_before_signin_pic
   :align: center
   :width: 20cm
   
   Вход с главной страницы Веб ГИС

В открывшемся диалоговом окне нажмите синюю кнопку **Войти через NextGIS ID**.

.. figure:: _static/ngweb_signin_nextgisid_ru.png
   :name: ngweb_signin_nextgisid_pic
   :align: center
   :width: 20cm
   
   Выбор входа по NextGIS ID

Вы будете перенаправлены на страницу авторизации my.nextgis.com. Введите имя пользователя или емейл, использованный при регистрации аккаунта, и пароль. 

.. figure:: _static/ngweb_nextgisid_ru.png
   :name: ngweb_nextgisid_pic
   :align: center
   :width: 12cm
   
   Страница входа NextGIS ID

После успешной авторизации вы будете возвращены на страницу Веб ГИС.

.. _ngw_home_page:

Главная страница
--------------------------------

После авторизации пользователь попадает на главную 
страницу, представленную на рисунке :numref:`ngweb_main_page_administrative_interface_pic`

.. figure:: _static/ngweb_main_page_administrative_interface_ru.png
   :name: ngweb_main_page_administrative_interface_pic
   :align: center
   :width: 25cm

   Главная страница

   Цифрами обозначены: 1 - Основное меню; 2 - Пользовательские настройки (Выход, Избранное и Смена языка); 3 – Строка поиска ресурсов в Веб ГИС; 4 - Описание основной группы ресурсов; 5 - Дочерние ресурсы; 6 - Кнопка добавления ресурсов; 7 - Операции, которые можно проводить с основной группой; 8 - кнопка техподдержки.

В блоке основного меню (см. :numref:`ngweb_main_page_administrative_interface_pic`, п.1) размещены следующие пункты:

* Ресурсы
* Панель управления
* Справка
* План и функции

   
В блоке "Описание" (см. :numref:`ngweb_main_page_administrative_interface_pic`, п.4) размещается такая информация, как тип ресурса, его владелец и другие данные в зависимости от типа пресурса.

Блок "Дочерние ресурсы" (см. :numref:`ngweb_main_page_administrative_interface_pic`, п.5) 
включает в себя перечень всех ресурсов, которые размещены в основной группе. В виде таблицы
доступны название и тип ресурса. Одновременно с этим есть кнопки различных действий над ресурсами (редактирование, удаления, предпросмотр, открытие таблицы атрибутов).

Кнопка создания ресурса (см. :numref:`ngweb_main_page_administrative_interface_pic`, п.6) вызывает всплывающее окно, в котором можно выбрать тип ресурса для создания.

В текущей версии в главную группу ресурсов возможно добавить следующие виды данных (см. :numref:`ngweb_main_page_administrative_interface_pic`, п.7):

* Группа ресурсов
* Веб-карта
* Векторный слой
* Растровый слой
* Группа трекеров
* Набор тайлов
* Подложка веб-карты
* Проект Collector

* Сервис OGC API - Features

* Слой PostGIS
* Соединение PostGIS

* Соединение TMS
* Слой TMS

* Сервис WFS
* Соединение WMS
* Cлой WMS
* Сервис WMS

В зависимости от вашей версии NGW также могут быть подключены:

* Библиотека маркеров SVG
* 3D модель
* 3D сцена
* 3D тайлсет

Блок операций (см. :numref:`ngweb_main_page_administrative_interface_pic`, п.7) позволяет изменять/удалять эту группу (основную группу удалить нельзя - она является корневой).

Возможные операции над ресурсами:

* Изменить
* Удалить

Также на главной странице расположены дополнительные ссылки:

* `Демо-проекты <https://docs.nextgis.ru/docs_ngcom/source/demoprojects.html>`_;
* Права пользователя - позволяет посмотреть, какие права имеет пользователь в данной ВебГИС. `Подробнее о правах доступа <https://docs.nextgis.ru/docs_ngcom/source/permissions.html>`_;
* Представление JSON.

.. _ngw_change_lang:

Переключение языка
------------------

Любой авторизованный пользователь может переключить язык интерфейса.
Для этого в правом верхнем углу экрана по иконке пользователя нужно перейти в раздел "Настройки" (:numref:`admin_select_lang_pic`)

.. figure:: _static/admin_settings_lang_rus.png
   :name: admin_settings_lang_pic
   :align: center
   :width: 10cm

   Переход в настройки

Для выбора доступны следующие языки (:numref:`admin_settings_lang_pic`):

* По умолчанию в браузере
* Русский
* Английский 
* Болгарский
* Итальянский
* Испанский
* Китайский
* Немецкий
* Португальский
* Французский
* Чешский


.. figure:: _static/admin_select_lang_rus_2.png
   :name: admin_select_lang_pic
   :align: center
   :width: 16cm

   Изменение языка интерфейса


.. _ngw_search_bar:

Поиск ресурсов
---------------

Для удобного поиска ресурсов и навигации в NextGIS Web существует строка поиска в верхней панели интерфейса (:numref:`admin_search_bar_pic`)
Поиск осуществляется по всей меющейся базе данных, независимо от того, в какой директории пользователь в данный момент находится.

.. figure:: _static/admin_search_bar_rus.png
   :name: admin_search_bar_pic
   :align: center
   :width: 10cm

   Поиск ресурсов в веб ГИС

.. _ngw_control_panel:

Панель управления
--------------------------------

В панель управления NextGIS Web можно попасть через основное меню (см. :numref:`ngweb_main_page_administrative_interface_pic`, п.1), выбрав соответствующий пункт. Панель управления представлена на :numref:`ngweb_control_panel`.

.. figure:: _static/ngweb_control_panel_new_ru.png
   :name: ngweb_control_panel
   :align: center
   :width: 9cm

   Панель управления

Панель управления позволяет выполнять следующие действия:

* `Управлять группами и пользователями NextGIS Web <https://docs.nextgis.ru/docs_ngweb/source/users.html>`_
* `Показать информацию о системе <https://docs.nextgis.ru/docs_ngweb/source/infowebgis.html>`_
* `Настроить кадастровые сервисы <https://docs.nextgis.ru/docs_ngweb/source/cadaster.html>`_
* `Добавить учетные записи пользователей <https://docs.nextgis.ru/docs_ngweb/source/collector.html>`_ для проектов Collector
* `Задать название веб ГИС <https://docs.nextgis.ru/docs_ngweb/source/look.html#ngw-name>`_
* `Настроить CORS <https://docs.nextgis.ru/docs_ngweb/source/cors.html>`_
* `Задать стилии CSS <https://docs.nextgis.ru/docs_ngweb/source/look.html#css>`_
* `Настроить пользовательские шрифты <https://docs.nextgis.ru/docs_ngweb/source/look.html#ngw-fonts>`_
* `Указать стартовую страницу NGW <https://docs.nextgis.ru/docs_ngweb/source/look.html#ngw-homepage>`_
* `Задать Логотип (в верхнем левом углу) <https://docs.nextgis.ru/docs_ngweb/source/look.html#ngw-css-logo>`_
* `Подключить метрики и аналитику <https://docs.nextgis.ru/docs_ngweb/source/analytics.html>`_
* Настроить категорию пользователей, которые имеют `права экспортировать данные <https://docs.nextgis.ru/docs_ngweb/source/look.html#ngw-res-export>`_
* `Настроить Трекинг <https://docs.nextgis.ru/docs_ngweb/source/trackers.html>`_
* `Настроить веб-карту <https://docs.nextgis.ru/docs_ngweb/source/webmap_set.html>`_
* `Управлять системами координат <https://docs.nextgis.ru/docs_ngweb/source/ngw_srs.html#ngw-srs>`_
* Персонализировать элементы интерфейса при помощи `модуля White Label <https://docs.nextgis.ru/docs_ngweb/source/label.html>`_ (для Веб ГИС, развёрнутой на своём сервере)

Ряд настроек доступен только для пользователей плана `Premium <http://nextgis.ru/nextgis-com/plans>`_ .



.. _ngw_view_resource:

Просмотр ресурсов
------------------

После авторизации пользователь попадает на главную страницу :numref:`ngweb_resource_group`

.. figure:: _static/resource_group_ru_2.png
   :name: ngweb_resource_group
   :align: center
   :width: 20cm

   Главная страница

Из списка ресурсов можно совершать ряд операций, нажимая на иконки в строке:

* |button_edit| перейти в редактирование ресурса
* |button_delete| удалить ресурс. 

В зависимости от типа ресурса могут быть доступны другие действия

* |button_preview| просмотр
* |button_open_web_map| открыть (для веб-карты)
* |button_open_feature_table| открыть таблицу атрибутов (для векторных слоёв)

Нажмите на строку с ресурсом, чтобы перейти на его страницу и увидеть информацию о ресурсе и список его дочерних ресурсов.

Отсюда также можно перейти в режим `редактирования ресурса <https://docs.nextgis.ru/docs_ngweb/source/edit_resource.html>`_.

.. figure:: _static/options_vector_layer_ru.png
   :name: ngweb_options_resource_group
   :align: center
   :width: 20cm
 
   Параметры векторного слоя

.. _ngw_feature_table:

Таблица объектов
-----------------

Ряд слоев (векторые, PostGIS) состоят из набора объектов. Для удобной работы с этими данными существуют Таблицы объектов.
Чтобы открыть таблицу слоя, нужно либо нажать на иконку таблицы в списке дочерних ресурсов (см. :numref:`feature_table_choice_from_group_pic`) либо пройти внутрь ресурса этого слоя и в блоке операций выбрать "Таблица объектов" (см. :numref:`ngweb_Object_table`).

.. figure:: _static/feature_table_choice_from_group_ru.png
   :name: feature_table_choice_from_group_pic
   :align: center
   :width: 20cm

   Выбор таблицы объектов в списке ресурсов

.. figure:: _static/feature_table_choice_rus_2.png
   :name: ngweb_Object_table
   :align: center
   :width: 20cm

   Выбор таблицы объектов на странице слоя


.. figure:: _static/map_and_tree_layers_rus_3.png
   :name: ngweb_admin_map_and_tree_layers_upload
   :align: center
   :width: 20cm

   Открытие таблицы объектов на веб-карте

Авторизованные пользователи могут `редактировать объекты через таблицу <https://docs.nextgis.ru/docs_ngweb/source/feature_table.html>`_.


.. _ngw_vector_export:

Экспорт векторных данных
-------------------------
  
Веб ГИС позволяет экспортировать данные в следующие форматы:

* :term:`GeoJSON`
* :term:`CSV`
* CSV для Microsoft Excel
* ESRI Shapefile
* AutoCAD DXF
* Mapinfo TAB
* MapInfo MIF/MID
* GeoPackage.


При экспорте в некоторые форматы создаются дополнительные файлы, например CSVT (описание полей) и PRJ (описание проекции) для CSV, CPG (кодовая страница) для ESRI Shapefile.

Для того, чтобы экспортировать данные:

#. Откройте ресурс Векторного слоя или Слоя PostGIS, данные которого вы хотите экспортировать;
#. Выберите пункт :menuselection:`Объекты --> Сохранить как` на правой панели :ref:`веб-интерфейса <ngw_admin_interface>`;
#. Укажите формат и кодировку данных;
#. При необходимости можно результат запаковать в ZIP архив (для ряда форматов это настройка по умолчанию)
#. Сохраните файл себе на устройство

В поле "Формат" следует указать требуемый формат данных:

.. figure:: _static/formats_ru.png
   :name: newformats_pic
   :align: center
   :width: 20cm    

   Поле "Формат"

Также можно выполнить экспорт объектов слоя (всех или некоторых) через `таблицу объектов <https://docs.nextgis.ru/docs_ngweb/source/feature_table.html>`_.
