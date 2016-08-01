.. sectionauthor:: Denis Rykov <denis.rykov@nextgis.com>

.. _ngw_install_centos7:

Установка в CentOS 7
====================

Данная инструкция проверена и работает в CentOS 7.
Для установки системы необходим Python 2.7.

.. warning:: 
   Для поддержки файлов названных на национальных языках (например, ru_RU.UTF-8) 
   необходимо, чтобы в системе была установлена соответствующая локаль.
   Проверку наличия локали можно командой: locale -a.
   Для добавления локали можно воспользоваться следующей командой: locale-gen 
   ru_RU.utf8.

Подготовка базы данных
----------------------

В официальном репозитории CentOS 7 нет PostGIS, поэтому необходимо
подключить сторонний репозиторий. Для этого перейдите по `адресу <http://yum.postgresql.org/repopackages.php>`_
и выберите необходимый пакет, соответствующий текущему релизу
PostgreSQL и установите его. Для версии PostgreSQL 9.5 это будет
выглядеть так:

.. code:: bash

    $ sudo yum install https://download.postgresql.org/pub/repos/yum/9.5/redhat/rhel-7-x86_64/pgdg-centos95-9.5-2.noarch.rpm

Список всех пакетов, доступных в данном репозитории, можно посмотреть
с помощью следующей команды:

.. code:: bash

    $ yum list | grep pgdg95

Устанавливаем PostgreSQL:

.. code:: bash

    $ sudo yum install postgresql95 postgresql95-server postgresql95-libs \
    postgresql95-contrib postgresql95-devel

Инициализируем базу данных и включаем автоматический запуск PostgreSQL
при старте системы:

.. code:: bash

    $ sudo /usr/pgsql-9.5/bin/postgresql95-setup initdb
    $ sudo systemctl start postgresql-9.5.service
    $ sudo systemctl enable postgresql-9.5.service

Создаем пользователя, который будет упомянут в качестве database.user в
config.ini (см. далее):

.. code:: bash

    $ sudo -u postgres createuser ngw_admin -P -e

Создаем базу, в которую будет развернут NGW, имя базы должно быть таким
же как и database.name в config.ini (см. далее):

.. code:: bash

    $ sudo -u postgres createdb -O ngw_admin --encoding=UTF8 db_ngw

Отредактируем параметры аутентификации в соответствующем файле:

.. code:: bash

    $ sudo nano /var/lib/pgsql/9.5/data/pg_hba.conf

Отредактируем его таким образом, чтобы в нём присутствовали следующие
строки (исправим метод аутентификации на ``md5``, если указан иной):

.. code:: bash

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5
    # IPv6 local connections:
    host    all             all             ::1/128                 md5

Не забудьте перезапустить PostgreSQL:

.. code:: bash

    $ sudo systemctl restart postgresql-9.5.service

Добавляем репозиторий с зависмостями для PostGIS:

.. code:: bash

    $ sudo yum install epel-release

Устанавливаем PostGIS:

.. code:: bash

    $ sudo yum install postgis2_95
    $ sudo -u postgres psql -d db_ngw -c 'CREATE EXTENSION postgis;'
    $ sudo -u postgres psql -d db_ngw -c \
    'ALTER TABLE geometry_columns OWNER TO ngw_admin;'
    $ sudo -u postgres psql -d db_ngw -c \
    'ALTER TABLE spatial_ref_sys OWNER TO ngw_admin;'
    $ sudo -u postgres psql -d db_ngw -c \
    'ALTER TABLE geography_columns OWNER TO ngw_admin;'

После этих операций будут созданы БД PostgreSQL с установленным в ней
:term:`PostGIS` и пользователь :abbr:`БД (база данных)`, который станет ее владельцем, а также 
таблиц ``geometry_columns``, ``georgaphy_columns``, ``spatial_ref_sys``.

Убедитесь, что функции PostGIS появились в базе:

.. code:: bash

    $ psql -h localhost -d db_ngw -U ngw_admin -c "SELECT PostGIS_Full_Version();"

Подготовка базового ПО
----------------------

Устанавливаем pip:

.. code:: bash

    $ sudo yum install python-pip

Устанавливаем virtualenv:

.. code:: bash

    $ sudo yum install python-virtualenv

Установливаем дополнительные инструменты:

.. code:: bash

    $ sudo yum install git gdal gdal-devel libxml2-devel libxslt-devel gcc-c++ \
    geos-devel proj-epsg zlib-devel libjpeg-turbo-devel dejavu-sans-fonts

Подготовка к установке NextGIS Web
----------------------------------

См. :ref:`ngw_install_prepare`.

Установка NextGIS Web
---------------------

Устанавливаем переменную окружения ``PATH``:

.. code:: bash

    $ export PATH=/usr/pgsql-9.5/bin:$PATH

Устанавливаем пакет NextGIS Web в режиме разработки, при этом будут установлены все необходимые пакеты:

.. code:: bash

    $ env/bin/pip install -e ./nextgisweb

При сборке пакетов может не хватить оперативной памяти (было замечено
на машине с 512 MB памяти при установке пакета ``lxml``), в этом
случае объём памяти можно увеличить за счёт swap файла,
`подробнее <http://stackoverflow.com/a/18335151/813758>`_.

Установка MapServer
-------------------

В репозитории нет MapServer, поэтому для пользователей CentOS 7
мы предварительно собрали необходимые пакеты:
`mapserver <http://nextgis.ru/programs/centos7/mapserver-7.0.0-1.el7.centos.x86_64.rpm>`_ и
`mapserver-python <http://nextgis.ru/programs/centos7/mapserver-python-7.0.0-1.el7.centos.x86_64.rpm>`_.

Скачиваем их и устанавливаем в систему:

.. code:: bash

    $ sudo yum install fribidi cairo fcgi harfbuzz httpd librsvg2
    $ sudo rpm -ivh mapserver-7.0.0-1.el7.centos.x86_64.rpm
    $ sudo rpm -ivh mapserver-python-7.0.0-1.el7.centos.x86_64.rpm


После чего копируем необходимые файлы в директорию виртуального
окружения, используемого для работы NextGIS Web.

.. code:: bash

    $ mkdir env/lib/python2.7/site-packages/mapscript.egg
    $ cp /usr/lib64/python2.7/site-packages/*mapscript* \
    env/lib/python2.7/site-packages/mapscript.egg
    $ echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

Создаем файл ``PKG-INFO``:

.. code:: bash

    $ mkdir env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO
    $ touch env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO

И указываем в нём используемую версию MapScript:

.. code:: bash

    $ echo `python -c "import mapscript; print \
    'Version: %s' % mapscript.MS_VERSION"` \
    > env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO

Для дальнейшей установки следуйте инструкциям раздела
:ref:`ngw_mapserver_install` и далее.
