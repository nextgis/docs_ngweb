.. sectionauthor:: Артём Светлов <artem.svetlov@nextgis.ru>

.. _ngw_launch:
    
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

В промышленной эксплуатации нужно использовать не pserve, а :ref:`uWSGI <ngw_uwsgi>`.

Для проверки работоспособности необходимо в веб-браузере набрать:

::

    http://0.0.0.0:6543

.. note: IP адрес 0.0.0.0 необходим для проверки на том же сервере, где развернут NextGIS Web. Если веб-браузер не установлен
   на сервере, то можно набрать команду в новой консоли 'curl http://0.0.0.0:6543'. Также можно на другом компьютере в той же
   сети в веб-браузере набрать http://<ip адрес сервера с NextGIS Web>:6543.

Должно открыться окно авторизации.

.. note: При запуске pserve через supervisor необходимо добавить настройку 
   environment=LANG=ru_RU.UTF-8 для поддержки русских имен в названии загружаемых 
   файлов.


Имя и пароль по умолчанию:
~~~~~~~~~~~~~~~~~~~~~~~~~~

* Имя: administrator
* Пароль: admin


.. _ngw_uwsgi:

Запуск через uWSGI + nginx
--------------------------

Для начала необходимо установить uWSGI:

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
    
Для запуска uWSGI через unix socket секция должна иметь следующий вид:
    
::
    
    [uwsgi]
    plugins = python
    lazy-apps = true

    master = true
    workers = 4
    no-orphans = true

    pidfile = /run/uwsgi/%n.pid
    socket = /run/uwsgi/%n.sock
    chmod-socket = 666

    logto = /var/log/uwsgi/%n.log
    log-date = true

    limit-post = 7516192768

    harakiri = 6000
    socket-timeout = 6000

    env = PASTE_CONFIG=/opt/ngw/development.ini
    env = LANG=ru_RU.UTF-8

    home = /opt/ngw/env
    mount = /ngw=/opt/ngw/nextgisweb/nextgisweb/uwsgiapp.py
    manage-script-name = true

.. note:: Соответствующие папки должны быть созданы. Для работы локали 
   (LANG=ru_RU.UTF-8) необходимо что бы в системе имелись соответсвующие файлы 
   (locale -a). Если локали нет, то ее необходимо добавить (locale-gen ru_RU.utf8). 
   Так же рекомендуется установить локаль системной (update-locale LANG=ru_RU.UTF-8).
   
Конфигурационный файл Nginx (исправления вносятся в соответствующий файл из папки /etc/nginx/sites-available/):

.. code:: bash

    server {
          listen                      80;
          client_max_body_size        6G;
          large_client_header_buffers 8 32k;

        location /ngw {
            uwsgi_read_timeout 600s;
            uwsgi_send_timeout 600s;

            include            uwsgi_params;
            uwsgi_pass         unix:/run/uwsgi/ngw.sock;
        }
    }


Другие варианты запуска
-----------------------
**Внимание, эти варианты запуска официально не поддерживаются**

При использовании FreeBSD может потребоваться отключить WSGI file
wrapper, так как он иногда работает некорректно. Для этого в этой же
секции:

::

    env = WSGI_FILE_WRAPPER=no

В зависимости от того, какой интерфейс требуется на выходе от
uwsgi. Тут есть некоторая путаница, связаная с тем, что uwsgi - это
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

Знака \| в конфиге быть не должно, надо написать, например, так:

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
    stderr_logfile=/var/log/supervisor/%(program_name)s_stderr.log
    stdout_logfile=/var/log/supervisor/%(program_name)s_stdout.log


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

К сожалению, при использовании этого модуля не работают определенные функции,
например, сжатие gzip на стороне apache. Более того они могут привести к
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

Для запуска при помощи nginx в файл конфигурации веб сервера Nginx необходимо добавить 
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


Для работы Ajax запросов необходимы настройки CORS:
    
::
    
    #
    # Wide-open CORS config for nginx
    #
    location / {
         if ($request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' '*';
            #
            # Om nom nom cookies
            #
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            #
            # Custom headers and headers various browsers *should* be OK with but aren't
            #
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
            #
            # Tell client that this pre-flight info is valid for 20 days
            #
            add_header 'Access-Control-Max-Age' 1728000;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            add_header 'Content-Length' 0;
            return 204;
         }
         if ($request_method = 'POST') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
         }
         if ($request_method = 'GET') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
         }
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
          client_max_body_size 6G;   # для больших файлов увеличиваем размер POST запроса
          large_client_header_buffers 8 32k; # для больших файлов увеличиваем буфер

          
          location / {
            uwsgi_read_timeout 600s; #для больших файлов необходимо поставить большее время
            uwsgi_send_timeout 600s;

            include            uwsgi_params;
            uwsgi_pass         unix:/tmp/ngw.socket;

            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
            
            proxy_buffer_size 64k; # для больших файлов увеличиваем буфер
            proxy_max_temp_file_size 0; # и размер временного файла ставим без огранчиений
	    proxy_buffers 8 32k;
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
	limit-post = 7516192768 # ограничение post запроса 7Гб
	harakiri = 6000	# таймаут на операцию 6000 с.
	socket-timeout = 6000 # таймаут на сокет 6000 с.


nginx + uwsgi (вариант 3)
~~~~~~~~~~~~~~~~~~~~~~~~~

::

	[app:main]
	use = egg:nextgisweb
	config = /opt/ngw/config.ini

	[server:main]
	use = egg:waitress#main
	host = 0.0.0.0
	port = 6543

	[uwsgi]
	plugins = python
	home = /opt/ngw/env
	module = nextgisweb.uwsgiapp
	env = PASTE_CONFIG=%p
	env = LANG=ru_RU.UTF-8
	socket = :6543
	protocol = uwsgi
	chmod-socket=777
	paste-logger = %p
	workers = 2
	threads = 4
	limit-post = 7516192768
	harakiri = 6000
	socket-timeout = 6000
	max-requests = 5000
	buffer-size = 32768

Сделать symlink на development.ini в папки:

/etc/uwsgi/apps-available/ngw.ini
/etc/uwsgi/apps-enabled/ngw.ini

::

	service uwsgi restart
	
Посмотреть лог на отсутствие ошибок:

::

	cat /var/log/uwsgi/app/ngw.log
