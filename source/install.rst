.. sectionauthor:: Артём Светлов <artem.svetlov@nextgis.ru>
.. sectionauthor:: Dmitry Baryshnikov <dmitry.baryshnikov@nextgis.com>

.. _ngw_install:    

Установка
=========

Данная инструкция проверена и работает в Ubuntu Server 14.04 LTS и выше.
Для установки системы необходим Python 2.7. 

.. warning:: 
   Для поддержки файлов названных на национальных языках (например, ru_RU.UTF-8) 
   необходимо, что бы в системе была установлена соответсвующая локаль.
   Проверку наличия локали можно командой: locale -a
   Для добавления локали можно воспользоваться следующей командой: locale-gen ru_RU.utf8

Подготовка базы данных
----------------------

Подключить репозиторий ubuntugis (см. `поддерживаемые
дистрибутивы <http://trac.osgeo.org/ubuntugis/wiki/SupportedDistributions>`_):

.. code:: bash

   apt-get update
   sudo apt-get install software-properties-common python-software-properties

**Для ubuntu server 14.04**

.. code:: bash
    
    sudo apt-add-repository ppa:ubuntugis/ubuntugis-unstable
    sudo apt-get update
    sudo apt-get upgrade

Установить PostgreSQL:

.. code:: bash

    sudo apt-get install postgresql-<version>

Создаем пользователя, который будет упомянут в качестве database.user в
config.ini (см. далее):

.. code:: bash

    sudo -u postgres createuser ngw_admin -P -e

после ввода пароля три раза говорим 'n'.

Создаем базу, в которую будет развернут NGW, имя базы должно быть таким
же как и database.name в config.ini (см. далее):

.. code:: bash

    sudo -u postgres createdb -O ngw_admin --encoding=UTF8 db_ngw
    sudo nano /etc/postgresql/9.3/main/pg_hba.conf

Отредактируем файл таким образом, чтобы в нём присутствовали следующие
строки (исправим метод аутентификации на ``md5``, если указан иной):

.. code:: bash

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5
    # IPv6 local connections:
    host    all             all             ::1/128                 md5


Не забудьте перезапустить сервис базы:

.. code:: bash

    sudo service postgresql restart

Установить PostGIS:

.. code:: bash

    sudo apt-cache search postgis

В полученном списке найдите пакет, подходящий для вашей версии
PostgreSQL, его имя должно иметь вид
postgresql-{version}-postgis-{version} и установите его:

.. code:: bash

    sudo apt-get install postgresql-9.3-postgis-2.1
    sudo -u postgres psql -d db_ngw -c 'CREATE EXTENSION postgis;'
    sudo -u postgres psql -d db_ngw -c 'ALTER TABLE geometry_columns OWNER TO ngw_admin;'
    sudo -u postgres psql -d db_ngw -c 'ALTER TABLE spatial_ref_sys OWNER TO ngw_admin;'
    sudo -u postgres psql -d db_ngw -c 'ALTER TABLE geography_columns OWNER TO ngw_admin;'

После этих операций будут созданы БД PostgreSQL с установленным в ней
:term:`PostGIS` и пользователь :abbr:`БД (база данных)`, который станет ее владельцем, а также 
таблиц ``geometry_columns``, ``georgaphy_columns``, ``spatial_ref_sys``.

Убедитесь, что функции PostGIS появились в базе:

.. code:: bash

    psql -d db_ngw -U ngw_admin -c "SELECT PostGIS_Full_Version();"

Если вы разворачиваете систему на чистом сервере, и вам надо сделать ещё
одну базу PostGIS для хранения данных, то можно включить доступ к ней из сети

.. code:: bash

    sudo su - postgres
    nano /etc/postgresql/9.3/main/pg_hba.conf
    добавляем в конец строку: host    all    all    192.168.0.0/16    md5
    маска 192.168.0.0/16 разрешает доступ к БД со всех IP адресов начинающихся с "192.168"

    nano /etc/postgresql/9.3/main/postgresql.conf
    делаем строку listen_addresses='*', и расскоментируем её.

.. code:: bash

    sudo service postgresql restart

Подготовка базового ПО
----------------------

Установить pip:

.. code:: bash

    sudo apt-get install python-pip

Установить virtualenv:

.. code:: bash

    sudo pip install virtualenv

Установить дополнительные инструменты:

.. code:: bash

    sudo apt-get install python-mapscript git libgdal-dev python-dev g++ \
    libxml2-dev libxslt1-dev gdal-bin libgeos-dev zlib-devel libjpeg-turbo8-dev

В случае доработки NextGIS Web может понадобится регистрация ключей. 
**Для большинства случаев ключи генерировать не нужно!** Это необходимо при
разработке.

Генерируем ключи для работы с GitHub (копируем и вставляем ключ в
настройки пользователя GitHub в `разделе SSH keys <https://github.com/settings/ssh>`_):

.. code:: bash

    mkdir ~/.ssh
    cd ~/.ssh
    ssh-keygen -t rsa -C "your@email.com"
    ssh-add ~/.ssh/id_rsa
    cat id_rsa.pub
    cd ~

Если включена двух-факторная авторизация, понадобится еще:

* `Закэшировать пароль <https://help.github.com/articles/caching-your-github-password-in-git/#platform-linux>`_
* `Сгенерировать access token <https://github.com/settings/applications#personal-access-tokens>`_
  и использовать его вместо пароля

Подготовка к установке NextGIS Web
----------------------------------

Создаём необходимые директории:

.. code:: bash

    mkdir -p ~/ngw/{data,upload}
    cd ~/ngw

Клонируем репозиторий:

.. code:: bash

    git clone https://github.com/nextgis/nextgisweb.git

Создаем виртуальное окружение virtualenv в папке ``~/ngw/env`` (папка
создастся сама после выполнения команды):

.. code:: bash

    virtualenv --no-site-packages env

Установка NextGIS Web
---------------------

Устанавливаем пакет NextGIS Web в режиме разработки, при этом будут
установлены все необходимые пакеты:

.. code:: bash

    env/bin/pip install -e ./nextgisweb



Установка MapServer
-------------------

Для работы модуля нужен MapScript, который в виртуальное окружение
стандартным способом не ставится, поэтому установим его вручную.

Устанавливаем необходимый пакет в систему:

.. code:: bash

    sudo apt-get install python-mapscript

После чего копируем необходимые файлы в директорию виртуального
окружения, используемого для работы NextGIS Web. На этом шаге возможны
как минимум 2 варианта в зависимости от того, в каком виде
устанавливается пакет python-mapscript в систему. Это зависит от
используемого дистрибутива.

Если вы используете Ubuntu, то для копирования системного MapScript в
виртуальное окружение (директория ``env``) можно воспользоваться
следующими командами:

.. code:: bash

    mkdir env/lib/python2.7/site-packages/mapscript.egg
    cp /usr/lib/python2.7/dist-packages/*mapscript* \ 
    env/lib/python2.7/site-packages/mapscript.egg
    echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

Если вы используете FreeBSD, то процесс будет несколько отличаться:
    
.. code:: bash

    cp -r `python -c "import mapscript, os.path; print \ 
    os.path.split(mapscript.__file__)[0]"` env/lib/python2.7/site-packages/mapscript.egg
    echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

Если вы используете Fedora/CentOS, то:

.. code:: bash

    mkdir env/lib/python2.7/site-packages/mapscript.egg
    cp /usr/lib/python2.7/site-packages/*mapscript* \ 
    env/lib/python2.7/site-packages/mapscript.egg
    echo "./mapscript.egg" > env/lib/python2.7/site-packages/mapscript.pth

Если сейчас выполнить команду:

.. code:: bash

    env/bin/pip freeze

то вы получите сообщение об ошибке:

::

    "Missing 'Version:' header and/or PKG-INFO file", mapscript [unknown version]

Для её исправления создаем файл ``PKG0-INFO``:

.. code:: bash

    mkdir env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO
    touch env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO

И указываем в нём используемую версию MapScript:

.. code:: bash

    echo `python -c "import mapscript; print 'Version: %s' % mapscript.MS_VERSION"` \
    > env/lib/python2.7/site-packages/mapscript.egg/EGG-INFO/PKG-INFO

Установка NextGIS Web MapServer
-------------------------------

Клонируем репозиторий: 

.. code:: bash

    git clone https://github.com/nextgis/nextgisweb_mapserver.git

Устанавливаем пакет в режиме разработки:

.. code:: bash

    env/bin/pip install -e ./nextgisweb_mapserver

Еще раз выполните команду:

.. code:: bash

    env/bin/pip freeze

чтобы убедиться, что ошибок нет.


Конфигурационный файл NextGIS Web
---------------------------------

Конфигурационный файл с параметрами по умолчанию может быть создан при помощи
команды ``nextgisweb-config``:

.. code:: bash

    env/bin/nextgisweb-config > config.ini

В результате будет создан конфигурационный файл ``config.ini``. В этот
текстовый файл нужно внести изменения в соответствии со своим
окружением. Назначение параметров указано в комментариях. Имя и пароль
пользователя, а так же путь к директории для хранения данных берутся
из команд выше. Необходимо убедиться, что правильно указаны следующие
параметры:


Пример конфигурационного файла NextGIS Web
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: 

	[file_upload]
	
	# Директория для временного хранения загруженных файлов
	# path =

	[pyramid]

	# Ключ, используемый для шифрования cookies (обязательно) 
	secret =  
	# HTML-справка 
	help_page = /home/trolleway/ngw/help.htm
	# Логотип системы 
	# logo = 
	# Значок для избранного 
	# favicon = 
	# Ссылка для редиректа, при заходе на / 
	# home_url = 

	[core]

	# Название системы 
	system.name = NextGIS Web
	# Полное название системы 
	system.full_name = Демонстрационная веб-гис
	# Имя сервера БД 
	database.host = localhost
	# Имя БД на сервере 
	database.name = db_ngw
	# Имя пользователя БД 
	database.user = ngw_admin
	# Пароль пользователя БД 
	database.password =  
	# Проверять подключение при запуске 
	# database.check_at_startup = 
	# Не загружать перечисленные пакеты 
	# packages.ignore = 
	# Не загружать перечисленные компоненты 
	# components.ignore = 
	# Директория для хранения данных 
	sdir = /home/trolleway/ngw/data
	# Локаль, используемая по-умолчанию
	locale.default = ru

	[file_storage]

	# Директория для хранения файлов 
	# path =

	[feature_layer]

	# Показывать атрибуты в идентификации 
	# identify.attributes = 

	[webmap]

	# Файл с описанием базовых слоёв 
	# basemaps = 
	# Bing Maps API-ключ 
	# bing_apikey = 
	# Чувствительность идентификации 
	# identify_radius = 
	# Ширина всплывающего окна 
	# popup_width = 
	# Высота всплывающего окна 
	# popup_height = 

	[wmsclient]


	[mapserver]

	# Список шрифтов в формате MAPFILE FONTSET 
	# fontset = 



Для генерации ключа для конфигурационного файла ``config.ini`` можно 
воспользоваться командой

.. code:: bash
	
	openssl rand -base64 16

.. note:
   В некоторых случаях необходимо указывать абсолютные пути к папкам, 
   параметр python %(here)s не во всех случаях действует.

Так же для работы команд pserve или pshell потребуется конфигурационный
файл paster, например ``development.ini``.

.. code:: bash

    nano development.ini

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

Интернационализация и локализация
---------------------------------

Поскольку скомпилированные файлы переводов не хранятся внутри
системы контроля версий, перед запуском необходимо их
скомпилировать (отдельно для каждого пакета), в противном случае
весь административный интерфейс будет на английском:

.. code:: bash

    env/bin/nextgisweb-i18n --package nextgisweb compile
    env/bin/nextgisweb-i18n --package nextgisweb_mapserver compile
    
Для установки локализации по-умолчанию для русского языка необходимо в конфигурационный файл (например, **config.ini**) добавить в секцию **core** следующую строку:

.. code:: ini

   locale.default = ru
   
Тогда при первом входе интерфейс будет на русском.   

Инициализация БД
----------------

Если по умолчанию планируется использовать в интерфейсе язык,
отличный от английского, то перед тем как выполнять первоначальную
инициализацию БД убедитесь, что вы скомпилировали файлы переводов и что
в настройке ``locale.default`` компонента ``core`` в конфигурационном
файле ``config.ini`` выставлен нужный язык, в противном случае ряд строк
будет отображаться на английском даже при принудительной смене языка
в административном интерфейсе.

Инициализация БД выполняется следующим образом:

.. code:: bash

    env/bin/nextgisweb --config config.ini initialize_db

В некоторых случаях, например при обновлении, может потребоваться
удалить все существующие в БД данные и инициализировать БД повторно:

.. code:: bash

    env/bin/nextgisweb --config config.ini initialize_db --drop


Миграция
--------

Миграция – это процедура по переносу данных и настроенной NextGIS Web между 
серверами. В ходе процедуры миграции создается резервная копия, в которую 
записывается:

* Всё содержимое базы данных NextGIS Web: информация о слоях, стили, аккаунты 
  пользователей, то есть всё, что настраивается в интерфейсе администратора.
* Векторные данные, которые были загружены через интерфейс администратора.
* Растровые данные, которые были загружены через интерфейс администратора. 

Файл config.ini в резервную копию не включаются, его надо переносить отдельно.

Для запуска процедуры миграции необходимо выполнять следующие команды:

.. code:: bash

	env/bin/nextgisweb --config config.ini backup file.ngwbackup
	env/bin/nextgisweb --config config.ini restore file.ngwbackup

Резервная копия – это ZIP-архив. Для отключения архивации резервной копии 
необходимо указать ключ —no-zip. При это будет создан новый каталог с указанным 
именем.

.. code:: bash

	env/bin/nextgisweb  --config "config.ini" backup "backup/ngwbackup" --no-zip

В ОС FreeBSD есть ошибка: поддержка sqlite не переносится virtualenv. Нужно 
вручную скопировать файл:

.. code:: bash

	cp /usr/local/lib/python2.7/site-packages/_sqlite3.so \
	env/lib/python2.7/site-packages/


Миграция выполняется в следующем порядке:

1. На старом сервере запускается процедура резервного копирования.

.. code:: bash

	env/bin/nextgisweb  --config "config.ini" backup "backup/ngwbackup" --no-zip

2. Если необходимо перенести базу PostGIS с геоданными, то со старого сервера 
   делается ее резервная копия программой pgAdminIII в формате tar.
3. На новом сервере устанавливаем NextGIS Web согласно инструкции (см. разд. 2).
4. На новом сервере создается база данных для NextGIS Web, и настраиваются  
   права доступа программой pgAdminIII.
5. На новом сервере в файле config.ini необходимо указать подключение к базе 
   NextGIS Web.

 
.. code::

	# Имя сервера БД 
	database.host = localhost
	# Имя БД на сервере 
	database.name = zapoved_ngw
	# Имя пользователя БД 
	database.user = user
	# Пароль пользователя БД 
	database.password = password


6. На новом сервере выполняем команду: 

.. code:: bash

	env/bin/nextgisweb  --config "config.ini" restore "backup/ngwbackup"

7. Запустите NextGIS Web. Должно работать всё, кроме слоёв PostGIS (при их  
   наличии).
8. Если необходимо перенести базу PostGIS с геоданными, то создается новая база 
   данных, в нее разворачивается резервная копия со старого сервера.
9. В настройках подключений PostGIS указывается новый адрес сервера. 

Если появляется ошибка "No module named pysqlite2" - значит при установке вы 
забыли перенести sqlite. Выполните нужную команду из инструкции по установке.


Обновление ПО
-------------

Для обновления ПО NextGIS Web необходимо выполнить команду:

.. code:: bash

	cd ~/ngw/nextgisweb
	git pull
	
Если в файле setup.py добавились какие-то зависимости, то следует выполнить:	

.. code:: bash

	sudo pip install -e ~/ngw/nextgisweb 
	
Если изменилась структура БД то следует выполнить:	

.. code:: bash

	cd ../
	env/bin/nextgisweb --config config.ini initialize_db

Кроме того, следует обновить пакет nextgisweb_mapserver:

.. code:: bash

	cd ./nextgisweb_mapserver
	git pull

После выполнения команд необходимо перезапустить ПО NextGIS Web либо перезапуском 
pserve, либо веб-сервера с модулем uWSGI.


Ошибки и предупреждения
-----------------------

В ходе работы ПО могут выдаваться диагностические сообщения в окно консоли, где 
запущен pserve или в лог:

.. code:: bash

    ault.py:471: SAWarning: Unicode type received non-unicode bind param value.
    processors[key](compiled_params[key])

Данное сообщение является несущественным.

Если предполагается работа с API из leaflet или OpenLayers, то на сервере нужно настроить технологию CORS.
