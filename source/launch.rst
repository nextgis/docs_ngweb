.. sectionauthor:: Артём Светлов <artem.svetlov@nextgis.ru>

.. _launch:
    
Запуск
======

Запуск через Pserve
-------------------

Для запуска NextGIS Web через Pserve необходимо выполнить команду:

.. code:: bash

    env/bin/pserve development.ini

Для автоматического запуска NextGIS Web при загрузке операционной системы 
необходимо отредактировать пользовательский скрипт автозапуска:

.. code:: bash

    sudo nano /etc/rc.local

и добавить в него строку:

.. code:: bash

    /home/zadmin/ngw/env/bin/pserve --daemon  /home/zadmin/ngw/production.ini

В промышленной эксплуатации нужно использовать не pserve, а :ref:`uWSGI <uwsgi>`.

Для проверки работоспособности необходимо в веб-браузере набрать:

::

    http://0.0.0.0:6543

Должно открыться окно авторизации.

.. note: При запуске pserve через supervisor необходимо добавить настройку environment=LANG=ru_RU.UTF-8 для поддержки русскихимен в названии загружаемых файлов.


Имя и пароль по умолчанию:
~~~~~~~~~~~~~~~~~~~~~~~~~~

* Имя: administrator
* Пароль: admin


.. _uwsgi:

Запуск через uWSGI
------------------

Для начал необходимо установить uWSGI:

.. code:: bash

   user@ubuntu:~/ngw$ source env/bin/activate
   (env)user@ubuntu:~/ngw$ pip install uwsgi
    
или через системный сервис:

.. code:: bash

   apt-get install uwsgi uwsgi-plugin-python uwsgi-emperor
 
К существующему конфигурационном ini-файлу paste добавляем секцию
``uwsgi``

::

    [uwsgi]
    module = nextgisweb.uwsgiapp
    env = PASTE_CONFIG=%p

При использовании FreeBSD может потребоваться отключить WSGI file
wrapper, так как он иногда работает некорректно. Для этого в этой же
секции:

::

    env = WSGI_FILE_WRAPPER=no
    
Для запуска uWSGI через unix socet секция должна иметь следующий вид:
    
::
    
    [uwsgi]
    home = /home/ngw_admin/ngw/env
    socket = /home/ngw_admin/uwsgi/ngw
    protocol=uwsgi
    chmod-socket=777
    master = true
    processes = 8
    threads = 4
    logto = /home/ngw_admin/logs/ngw.log
    log-slow = 1000
    paste = config:%p
    paste-logger = %p
    env=LANG=ru_RU.UTF-8

.. note:: Соответсвующие папки должны быть созданы

Далее, в зависимости от того, какой интерфейс требуется на выходе от
uwsgi. Тут есть некоторая путаница, связаная с тем, что uwsgi это
одновременно и протокол и программа. Ниже речь идет именно о протоколе.

HTTP:

::

    socket = host:port | :port
    protocol = http

uWSGI:

::

    socket = host:port | :port | /path/to/socket
    protocol = uwsgi

FastCGI:

::

    socket = host:port | :port | /path/to/socket
    protocol = fastcgi

Знака \| в конфиге быть не должно, надо написать например так:

::

    socket =  :6543    

При использовании сокета в файловой системе права на него могут быть
выставлены через параметр chmod:

::

    chmod = 777

Количество процессов задается параметром ``workers``, а количество
потоков в процессе - параметром ``thread``. В примере ниже будет
запущено 2 процесса с 4 потоками в каждом:

::

    workers = 2
    threads = 4

Вариант с отдельным процессами более безопасный, но и более
ресурсоемкий.

Запуск uwsgi осуществляется командой ``uwsgi file.ini``, причем все
переменные могут быть так же переопределены из командной строки,
например так: ``uwsgi --workers=8 file.ini``. В таком же виде uwsgi
можно запускать и через supervisor, например так:

::

    [program:nextgisweb]
    command = /path/to/uwsgi /path/to/file.ini
    
supervisor + uwsgi
~~~~~~~~~~~~~~~~~~

Для запуска через supervisor + uWSGI без использования веб-сервера конфигурация 
должна иметь следующий вид:
    
::    

   [uwsgi]
   module = nextgisweb.uwsgiapp
   lazy = yes
   env = PASTE_CONFIG=%p
   env = PATH=/home/ngw_admin/ngw/env/bin:/bin:/usr/sbin:/usr/bin
   env = LANG=ru_RU.UTF-8
   virtualenv = /home/ngw_admin/ngw/env
   protocol = http
   socket = :8080
   workers = 4 # количество потоков обработки подключений
   limit-post = 4831838208 # максимальный размер файла

Конфигурация supervisor может иметь следующий вид:
    
::
    
    [program:ngw]
    command = /home/ngw_admin/ngw/env/bin/uwsgi /home/ngw_admin/ngw/production.ini
    user = ngw_admin
    environment=LANG=ru_RU.UTF-8


apache + mod\_uwsgi
~~~~~~~~~~~~~~~~~~~

При наличии модуля ``mod_uwsgi`` uwsgi можно подключить при помощи такой
конструкции:

::

    <Location /nextgisweb>
        SetHandler uwsgi-handler
        uWSGISocket /path/to/socket
    </Location>

В этом случае для коммуникации между uwsgi и apache используется сокет в
файловой системе, то есть в секции ``[uwsgi]`` должно быть:

::

    socket = /path/to/socket
    protocol = uwsgi

К сожалению, при использовании этого модуля не работают всякие фишки,
вроде сжатия gzip на стороне apache. Более того они могут привести к
совершенно неожиданным последствиям.

apache + mod\_proxy\_uwsgi
~~~~~~~~~~~~~~~~~~~~~~~~~~

При наличии модуля ``mod_proxy_uwsgi`` uwsgi можно подключить при помощи
такой конструкции:

::

    <Location /nextgisweb>
        ProxyPass uwsgi://localhost:10001
    </Location>

Порт приходится использовать из-за того, что ``mod_proxy`` в apache не
поддерживает сокеты из файловой системы. То есть в этом случае в
``[uwsgi]`` должно быть что-то вроде:

::

    socket = localhost:10001
    protocol = uwsgi
    
nginx + uwsgi
~~~~~~~~~~~~~

Для запуска при помощи nginx в файл конфигурации сервера необходимо добавить 
следующие строки.

В случае запуска uWSGI на TCP порту:    

:: 

    location /path_to_ngw_instance/ {
        include uwsgi_params;
	    uwsgi_pass 127.0.0.1:6543;
    }
    
    
В случае запуска uWSGI на unix порту:    

:: 

    location /path_to_ngw_instance/ {
        include uwsgi_params;
        uwsgi_pass unix:///home/ngw_admin/uwsgi/ngw;
    }


nginx + uwsgi (вариант 2)
~~~~~~~~~~~~~~~~~~~~~~~~~

Создаем файл с настройками:  

::

	sudo touch /etc/nginx/sites-available/ngw.conf

содержание:  

::

     server {
          listen                 6555;
          location / {
            uwsgi_read_timeout 600;
            uwsgi_send_timeout 600;

            include            uwsgi_params;
            uwsgi_pass         unix:/tmp/ngw.socket;

            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
        }
    }


Setup uWSGI

::

	[app:main]
	use = egg:nextgisweb
	
	# путь к основному конфигурационному файлу
	config = /opt/ngw/config.ini
	
	# путь к конфигурационному файлу библиотеки logging
	# logging = %(here)s/logging.ini
	
	# полезные для отладки параметры
	# pyramid.reload_templates = true
	# pyramid.includes = pyramid_debugtoolbar
	
	[server:main]
	use = egg:waitress#main
	host = 0.0.0.0
	port = 6543
	
	[uwsgi]
	plugins = python
	home = /opt/ngw/env
	module = nextgisweb.uwsgiapp
	env = PASTE_CONFIG=%p
	socket = /tmp/ngw.socket
	protocol = uwsgi
	chmod-socket=777
	paste-logger = %p
	workers = 8
