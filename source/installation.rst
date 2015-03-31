.. sectionauthor:: Артём Светлов <artem.svetlov@nextgis.ru>

.. _installation:

Эта инструкция проверена и работает в Ubuntu Server 13.10, Ubuntu Server
12.04 LTS, Ubuntu Desktop 13.04, Ubuntu Server 14.04 LTS, 14.10

Установка
===========

Для установки системы необходим Python 2.7:

Подготовка базы данных
~~~~~~~~~~~~~~~~~~~~~~

Подключить репозиторий ubuntugis (`поддерживаемые
дистрибутивы <http://trac.osgeo.org/ubuntugis/wiki/SupportedDistributions>`__):

.. code:: bash

    sudo apt-add-repository ppa:ubuntugis/ppa
    sudo apt-get update
    sudo apt-get upgrade

Установить PostgreSQL:

.. code:: bash

    sudo apt-get install postgresql

Создаем пользователя, который будет упомянут в качестве database.user в
config.ini (см. далее):

.. code:: bash

    sudo su postgres -c "createuser ngw_admin -P -e"

после ввода пароля три раза говорим 'n'.

Создаем базу в которую будет развернут NGW, имя базы должно быть таким
же как и database.name в config.ini (см. далее):

.. code:: bash

    sudo su postgres -c "createdb -O ngw_admin --encoding=UTF8 db_ngw"
    sudo nano /etc/postgresql/9.1/main/pg_hba.conf

Отредактируем строку ``local   all   all   peer`` и приведём её к виду:
``local   all   all   md5``

Не забудьте перезапустить сервис базы:

.. code:: bash

    sudo service postgresql restart

Установить PostGIS:

.. code:: bash

    sudo apt-cache search postgis

В полученном списке найдите пакет подходящий для вашей версии
PostgreSQL, его имя должно иметь вид
postgresql-{version}-postgis-{version} и установите его:

.. code:: bash

    sudo apt-get install postgresql-9.1-postgis-2.0
    sudo su - postgres -c "psql -d db_ngw -c 'CREATE EXTENSION postgis;'"
    sudo su - postgres -c "psql -d db_ngw -c 'ALTER TABLE geometry_columns OWNER TO ngw_admin'"
    sudo su - postgres -c "psql -d db_ngw -c 'ALTER TABLE spatial_ref_sys OWNER TO ngw_admin'"
    sudo su - postgres -c "psql -d db_ngw -c 'ALTER TABLE geography_columns OWNER TO ngw_admin'"

После этих операций будут созданы БД PostgreSQL с установленным в ней
PostGIS и пользователь БД, который будет владельцем БД и таблиц
``geometry_columns``, ``georgaphy_columns``, ``spatial_ref_sys``.

Убедитесь, что функции PostGIS появились в базе:

.. code:: bash

    psql -d db_ngw -U ngw_admin -c "SELECT PostGIS_Full_Version();"

Если вы разворачиваете систему на чистом сервере, и вам надо сделать ещё
одну базу PostGIS для хранения данных, то включаем доступ к ней из сети

.. code:: bash

    sudo su - postgres
    nano /etc/postgresql/9.3/main/pg_hba.conf
    делаем строку host    all     all     md5

    nano /etc/postgresql/9.3/main/postgresql.conf
    делаем строку listen_adress="*", и расскоментируем её.

.. code:: bash

    sudo service postgresql restart

Подготовка базового ПО
~~~~~~~~~~~~~~~~~~~~~~

Установить pip:

.. code:: bash

    sudo apt-get install python-pip

Установить virtualenv:

.. code:: bash

    sudo pip install virtualenv

Установить дополнительные инструменты:

.. code:: bash

    sudo apt-get install python-mapscript git libgdal-dev python-dev g++ libxml2-dev libxslt1-dev gdal-bin

Для большинства случаев ключи генерировать не нужно! Это необходимо при
разработке.

Генерируем ключи для работы с GitHub (копируем и вставляем ключ в
настройки пользователя GitHub в разделе SSH keys,
https://github.com/settings/ssh):

.. code:: bash

    mkdir ~/.ssh
    cd ~/.ssh
    ssh-keygen -t rsa -C "your@email.com"
    ssh-add ~/.ssh/id_rsa
    cat id_rsa.pub
    cd ~

Если включена двух-факторная авторизация, понадобится еще:

-  `Закэшировать
   пароль <https://help.github.com/articles/caching-your-github-password-in-git/#platform-linux>`__
-  `Сгенерировать access
   token <https://github.com/settings/applications#personal-access-tokens>`__
   и использовать его вместо пароля

Подготовка к установке NextGIS Web
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Создаем рабочую папку ``~/ngw``:

.. code:: bash

    mkdir ~/ngw
    mkdir ~/ngw/data
    mkdir ~/ngw/data/upload
    mkdir ~/ngw/backup   
    mkdir ~/ngw/data_storage
    cd ~/ngw

Клонируем репозиторий:

.. code:: bash

    git clone https://github.com/nextgis/nextgisweb.git

Создаем виртуальное окружение virtualenv в папке ``~/ngw/env`` (папка
создастся сама после выполнения команды):

.. code:: bash

    virtualenv --no-site-packages env

Установка NextGIS Web
~~~~~~~~~~~~~~~~~~~~~

Устанавливаем пакет NextGIS Web в режиме разработки, при этом будут
установлены все необходимые пакеты:

.. code:: bash

    env/bin/pip install -e ./nextgisweb

Установка NextGIS Web MapServer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Установка nextgisweb\_mapserver подробно описана
`тут <https://github.com/nextgis/nextgisweb_mapserver>`__.

Конфигурационный файл
~~~~~~~~~~~~~~~~~~~~~

Конфигурационный с параметрами по-умолчанию может быть создан при помощи
команды ``nextgisweb-config``:

.. code:: bash

    env/bin/nextgisweb-config > config.ini

В результате будет создан конфигурационный файл ``config.ini``. В этот
текcтовый файл нужно внести изменения в соответствии со своим
окружением. Назначение параметров указано в комментариях. Имя и пароль
пользователя, а так же пути к папкам хранения и загрузки файлов берутся
из команд выше. Необходимо убедиться, что правильно указаны следующие
параметры (значения приведены из примерах выше):

-  database.name = db\_ngw
-  database.user = ngw\_admin
-  database.password =
-  secret =
-  path = /home/sim/ngw/data/upload (секция [file\_upload])
-  path = /home/sim/ngw/data\_storage (секция [file\_storage])

Необходимо указывать абсолютные пути к папкам, %(here)s на данный момент
не действует.

Так же для работы команд pserve или pshell потребуется конфигурационный
файл paster, например ``development.ini``.

.. code:: bash

    gedit development.ini

Содержание:

::

    [app:main]
    use = egg:nextgisweb

    # путь к основному конфигурационному файлу
    config = %(here)s/config.ini

    # путь к конфигурационному файлу библиотеки logging
    # logging = %(here)s/logging.ini

    # полезные для отладки параметры
    # pyramid.reload_templates = true
    # pyramid.includes = pyramid_debugtoolbar

    [server:main]
    use = egg:waitress#main
    host = 0.0.0.0
    port = 6543

Если предполагается работа в сети без доступа к Интернету, то в файле
/nextgisweb/nextgisweb/webmap/basemaps.json нужно удалить записи про
подложки Google.

Инициализация БД
~~~~~~~~~~~~~~~~

Инициализация БД выполняется следующим образом:

.. code:: bash

    env/bin/nextgisweb --config config.ini initialize_db

В некоторых случаях, например при обновлении, может потребоваться
удалить все существующие в БД данные и инициализировать БД повторно:

.. code:: bash

    env/bin/nextgisweb --config config.ini initialize_db --drop

Запуск веб-сервера pserve:
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: bash

    env/bin/pserve development.ini

Автозапуск

.. code:: bash

    sudo nano /etc/rc.local

Добавить строку:

.. code:: bash

    /home/zadmin/ngw/env/bin/pserve --daemon  /home/zadmin/ngw/production.ini

В промышленной эксплуатации нужно использовать не pserve, а uWSGI. Далее
смотри
`DEPLOY.md <https://github.com/nextgis/nextgisweb/blob/2/DEPLOY.md>`__

Для проверки работоспособности наберите тут же в браузере:

::

    http://0.0.0.0:6543

Должно открыться окно авторизации.

Обновление
----------

.. code:: bash

    cd ~/ngw/nextgisweb
    git pull
    cd ..
    env/bin/pip install -e nextgisweb
    env/bin/nextgisweb --config config.ini initialize_db

Другое
------

Авторизация
~~~~~~~~~~~

Имя и пароль по умолчанию
^^^^^^^^^^^^^^^^^^^^^^^^^

-  Имя: administrator
-  Пароль: admin


