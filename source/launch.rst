.. sectionauthor:: Artem Svetlov <artem.svetlov@nextgis.ru>

.. _ngw_launch:
    
Launch
======

Launch using Pserve
--------------------

To launch NextGIS Web using Pserve run a command:

.. code:: bash

    env/bin/pserve development.ini

To launch NextGIS Web automatically with a launch of operation system edit a user script for autolaunch:

.. code:: bash

    sudo nano /etc/rc.local

and add the following string to the file:

.. code:: bash

    /home/zadmin/ngw/env/bin/pserve --daemon  /home/zadmin/ngw/production.ini

In production you should use :ref:`uWSGI <ngw_uwsgi>` instead of pserve.

To test if application is running go to the following address in the browser:

::

    http://0.0.0.0:6543

An authentication page will be open.

.. note: If pserve is launched using supervisor you should add a setting 
   environment=LANG=ru_RU.UTF-8 to support Russian names for uploaded 
   files.


Default login and password:
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Login: administrator
* Password: admin


.. _ngw_uwsgi:

Launch using uWSGI + nginx
--------------------------

At first you need to install uWSGI:

.. code:: bash

   user@ubuntu:~/ngw$ source env/bin/activate
   (env)user@ubuntu:~/ngw$ pip install uwsgi
    
or using system service:

.. code:: bash

   apt-get install uwsgi uwsgi-plugin-python uwsgi-emperor
 
Then you need to add an ``uwsgi`` section to existing .ini file:


::

    [uwsgi]
    module = nextgisweb.uwsgiapp
    env = PASTE_CONFIG=%p

    
To launch uWSGI using unix socket the uwsgi section should look like:
    
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

.. note:: Corresponding folders should be already created. To use locale  
   (LANG=ru_RU.UTF-8) required files should be present in system 
   (locale -a). If locale is absent you need to add it (locale-gen ru_RU.utf8). 
   Also it is recommended to set locale as system (update-locale LANG=ru_RU.UTF-8).

Nginx configuration file:

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


Other launch options
--------------------

**Important: these options are not officially supported.**

When using FreeBSD you may need to disable WSGI file wrapper, as it sometimes does not work properly. To do this add the following string to that section:

::

    env = WSGI_FILE_WRAPPER=no

The following steps will depend on what interface is required as an output of uwsgi. There is some confusion related to the fact that uwsgi is both protocol and program. Here we are talking about the protocol.

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

The sign \| should not be present in the configuration file. For example you can write:

::

    socket =  :6543    

When using socket you can set file system permissions using chmod parameter:

::

    chmod = 777

The number of processes is set with ``workers`` parameters. The number of threads for a process is set with a ``thread`` parameter. The example below shows a launch of 2 processes with 4 threads per process:

::

    workers = 2
    threads = 4

An option with separate processes is more safe but it consumes more resources.

Launch of uwsgi is executed using a command ``uwsgi file.ini``, and all variables could be redefined in command line. For example : ``uwsgi --workers=8 file.ini``. You can launch uwsgi the same way using supervisor, for example:

::

    [program:nextgisweb]
    command = /path/to/uwsgi /path/to/file.ini
    
supervisor + uwsgi
~~~~~~~~~~~~~~~~~~~

To launch supervisor + uWSGI without web server configuration file should look like:
    
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
   workers = 4 # the number of threads for processing of connections
   limit-post = 4831838208 # maximum file size

Configuration file for supervisor should look like:
    
::
    
    [program:ngw]
    command = /home/ngw_admin/ngw/env/bin/uwsgi /home/ngw_admin/ngw/production.ini
    user = ngw_admin
    environment=LANG=ru_RU.UTF-8
    stderr_logfile=/var/log/supervisor/%(program_name)s_stderr.log
    stdout_logfile=/var/log/supervisor/%(program_name)s_stdout.log


apache + mod\_uwsgi
~~~~~~~~~~~~~~~~~~~~

If module ``mod_uwsgi`` is available you can enable uwsgi with the following configuration:

::

    <Location /nextgisweb>
        SetHandler uwsgi-handler
        uWSGISocket /path/to/socket
    </Location>

In this case a file system socket is used for communication between uwsgi and apache, so section ``[uwsgi]`` should have the following strings:

::

    socket = /path/to/socket
    protocol = uwsgi

Unfortunatelly when using this module not all functions are available, for example gzip compression at the apache side will be unavailable. Moreover this can cause unexpected consequences.

apache + mod\_proxy\_uwsgi
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If module ``mod_proxy_uwsgi`` is available you can enable uwsgi with the following configuration:

::

    <Location /nextgisweb>
        ProxyPass uwsgi://localhost:10001
    </Location>

You need to use the port because ``mod_proxy`` in apache doesn't support file system sockets. So in this case the ``[uwsgi]`` section should contain something like:

::

    socket = localhost:10001
    protocol = uwsgi
    
nginx + uwsgi
~~~~~~~~~~~~~~

To launch using nginx you need to add the following strings to Nginx configuration file.

In case uWSGI is launched on TCP-port:    

:: 

    location /path_to_ngw_instance/ {
        include uwsgi_params;
	    uwsgi_pass 127.0.0.1:6543;
    }
    
    
In case uWSGI is launched on unix-port:    

:: 

    location /path_to_ngw_instance/ {
        include uwsgi_params;
        uwsgi_pass unix:///home/ngw_admin/uwsgi/ngw;
    }


To work with Ajax requests you should perform CORS setiings:
    
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


nginx + uwsgi (option 2)
~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a file with configuration:  

::

	sudo touch /etc/nginx/sites-available/ngw.conf

contents:  

::

     server {
          listen                 6555;
          client_max_body_size 6G;   # for large files increase POST request size
          large_client_header_buffers 8 32k; # for large files increase buffer size

          
          location / {
            uwsgi_read_timeout 600s; #for large files set longer timeout
            uwsgi_send_timeout 600s;

            include            uwsgi_params;
            uwsgi_pass         unix:/tmp/ngw.socket;

            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
            
            proxy_buffer_size 64k; # for large files increase buffer size
            proxy_max_temp_file_size 0; # and a temporary file size is set to infinite
	    proxy_buffers 8 32k;
        }
    }


Setup uWSGI

::

	[app:main]
	use = egg:nextgisweb
	
	# a path to the main configuration file
	config = /opt/ngw/config.ini
	
	# a path to logging library configuration file
	# logging = %(here)s/logging.ini
	
	# parameters useful for debugging
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
	limit-post = 7516192768 # POST request limit 7GB
	harakiri = 6000	# operation timeout 6000 seconds
	socket-timeout = 6000 # socket timeout 6000 seconds


nginx + uwsgi (option 3)
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

Create symlink to development.ini in folders:

/etc/uwsgi/apps-available/ngw.ini
/etc/uwsgi/apps-enabled/ngw.ini

::

	service uwsgi restart
	
Lookup log for error messages:

::

	cat /var/log/uwsgi/app/ngw.log
	
