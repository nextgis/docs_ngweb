.. sectionauthor:: Наталья Барышникова <nshelekhova@gmail.com>

.. _ngw_extension:

Модули расширения
================================

Общие сведения
-----------------

Имеются следующие модули расширения для увеличения функциональных возможностей программного обеспечения:

1. Rosavto - добавляется возможность выбора подложки по умолчанию и просмотр перечня 
   имеющихся подложек, возможность выбора из готового списка адресов сервисов.

2. Compulink - добавление к стандартному функционалу NextGIS Web справочника (ключ - значение),
   возможность экспорта векторных слоев в разные форматы (ESRI шейп-файл, MapInfo tab и др.)

3. Rekod - добавляется поддержка наборов файлов, позволяет загружать любые файлы, 
   не обязательно геопространственные.

4. Forest violations - добавляется возможность фильтрации по атрибутам и пространственному 
   охвату.

5. Mapnik - добавляется отрисовка карт при помощи Mapnik рендерера.

6. QGIS - добавляется отрисовка карт помощи QGIS рендерера.

7. MapServer - добавляется отрисовка карт при помощи MapServer рендерера.

8. Log - добавляется возможность для записи различных сообщений в логе.

9. Mobile debug - добавляется возможность для получения и систематизации отладочной 
   информации NextGIS Mobile SDK.
 
Добавление нового модуля расширения
------------------------------------

Рассмотрим пример добавления нового модуля расширения на базе Rekod. 
Для этого необходимо выполнить ряд шагов.

1. Выполнить команду, которая скачает исходные коды модуля на сервер, где установлен NextGIS Web: 

.. code-block:: bash

   git clone https://github.com/nextgis/nextgisweb_rekod.git

2. Установить модуль расширения в NextGIS Web: 

.. code-block:: bash

   env/bin/pip install -e ./nextgisweb_rekod

3. Создать необходимые директории (опционально):

.. code-block:: bash

   mkdir file_bucket

4. Изменить файл конфигурации (опционально):

.. code-block:: bash

   nano config.ini

Пример внесения изменений файл конфигурации см. ниже:

.. code-block:: bash

   [file_bucket]
   path =  /home/username/ngw/file_bucket

5. Переинициализировать базу данных NextGIS Web:

.. code-block:: bash

   env/bin/nextgisweb --config config.ini initialize_db 

6. Перезапустить сервис NextGIS Web, чтобы изменения вступили в силу.

.. _`ngw_install_qgis`:

Установка NextGIS Web QGIS
----------------------------------
Для установки модуля рендеринга с помощью QGIS необходимо, чтобы в системе уже был установлен QGIS версии 2.8 и выше.

.. code-block:: bash

    cd ~/ngw
    git clone git@github.com:nextgis/nextgisweb_qgis.git
    source env/bin/activate
    pip install -e nextgisweb_qgis/

QGIS и зависимости PyQT4 не перечисляются в ``setup.py`` потому что их сложно устанавливать в virtualenv. Поэтому просто копируем эти библиотеки из системных пакетов в virtualenv. Обычно они находятся в пакетах ``python-sip``, ``python-qt4`` и ``python-qgis``.

.. code-block:: bash

    # DST should point to virtualenv site-packages directory.
    # If it is point to another place you have to modify DST definition.
    # For example: DST=`python -c "import sys; print sys.path[-2]"`
    DST=`python -c "import sys; print sys.path[-1]"`
    echo $DST
    cp `/usr/bin/python -c "import sip; print sip.__file__"` $DST
    cp -r `/usr/bin/python -c "import PyQt4, os.path; print os.path.split(PyQt4.__file__)[0]"` $DST
    cp -r `/usr/bin/python -c "import qgis, os.path; print os.path.split(qgis.__file__)[0]"` $DST

Для версии QGIS 2.16 и выше:

.. code-block:: bash

    # Only for latest QGIS version (2.16 and higher)
    export PYTHONPATH=$PYTHONPATH:/usr/share/qgis/python
    cp -r `/usr/bin/python -c "import PyQt, os.path; print os.path.split(PyQt.__file__)[0]"` $DST
    
Настройки uWSGI
~~~~~~~~~~~~~~~

Необходимо добавить параметр `--lazy-apps`, в этом случае приложение будет загружено после основного и каждый воркер получит свой поток.

.. code-block:: bash

   [uwsgi]
   lazy-apps = True

Имейте в виду, что существует так же более старый параметр `lazy` использование которого не рекомендуется.

Если вы получаете ошибку: `ERROR: Auth db directory path could not be created` то вам нужно указать папку где находится существующий или создаётся заново файл qgis-auth.db. Эта папка должна иметь права на запись для пользователя процесса uwsgi. Например:

.. code-block:: bash

   [uwsgi]
   env = QGIS_AUTH_DB_DIR_PATH=/var/www

Если у вас проблемы с кириллицей в подписях, задайте системную переменную:

.. code-block:: bash

   environment = LC_ALL="en_US.UTF-8"

