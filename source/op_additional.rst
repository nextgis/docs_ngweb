Приложения
==========

Примеры файлов docker-compose.yaml
--------------------------------------

Standard Edition 3.1.0
~~~~~~~~~~~~~~~~~~~~~~

.. code:: yaml

   x-shared: &shared
     # Adjust the URLs below according to your network and reverse proxy
     # configuration. Initial values only work properly when accessing from
     # the server as localhost.
     NEXTGISWEB_URL: "http://hostname.localhost:8080"
     NEXTGISID_URL: "http://hostname.localhost:8081"

     NEXTGISID_INSTANCE_NGID: "00000000-0000-0000-0000-000000000000"
     NEXTGISID_ADMINISTRATOR_NGID: "00000000-0000-0000-0000-000000000000"

   services:
     app:
       image: cr.nextgis.com/nextgisweb/std/app:3.1.0
       volumes:
         - { type: volume, source: data_app, target: /opt/ngw/data/app }
         - { type: volume, source: config_app, target: /opt/ngw/config/app }
         - { type: volume, source: secret, target: /opt/ngw/secret }
         - { type: volume, source: backup, target: /opt/ngw/backup }
       depends_on: [postgres]
       restart: unless-stopped
       ports:
         - 8080:8080
       environment:
         <<: *shared
         NEXTGISWEB__CORE__LOCALE__DEFAULT: "ru"

     postgres:
       image: cr.nextgis.com/nextgisweb/std/postgres:3.1.0
       volumes:
         - { type: volume, source: data_postgres, target: /opt/ngw/data/postgres }
         - { type: volume, source: config_postgres, target: /opt/ngw/config/postgres }
         - { type: volume, source: secret, target: /opt/ngw/secret }
       restart: unless-stopped

     archivist:
       image: cr.nextgis.com/nextgisweb/std/archivist:3.1.0
       volumes:
         - { type: volume, source: data_app, target: /opt/ngw/data/app }
         - { type: volume, source: data_postgres, target: /opt/ngw/data/postgres }
         - { type: volume, source: data_ngid, target: /opt/ngid/data/ngid }
         - { type: volume, source: config_app, target: /opt/ngw/config/app }
         - { type: volume, source: config_postgres, target: /opt/ngw/config/postgres }
         - { type: volume, source: config_ngid, target: /opt/ngid/config/ngid }
         - { type: volume, source: secret, target: /opt/ngw/secret }
         - { type: volume, source: backup, target: /opt/ngw/backup }
       restart: unless-stopped

     ngid:
       image: cr.nextgis.com/nextgisweb/std/ngid:3.1.0
       volumes:
         - { type: volume, source: data_ngid, target: /opt/ngid/data/ngid }
         - { type: volume, source: config_ngid, target: /opt/ngid/config/ngid }
         - { type: volume, source: secret, target: /opt/ngid/secret }
       restart: unless-stopped
       ports:
       - 8081:8080
       environment:
         <<: *shared
         NGID_ONPREMISE_CONFIGURATION: |-
           eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9....

   volumes:
     data_app: {}
     data_ngid: {}
     data_postgres: {}
     config_app: {}
     config_ngid: {}
     config_postgres: {}
     secret: {}
     backup: {}

Extended Edition 3.1.0
~~~~~~~~~~~~~~~~~~~~~~

.. code:: yaml

   x-shared: &shared
     # Adjust the URLs below according to your network and reverse proxy
     # configuration. Initial values only work properly when accessing from
     # the server as localhost.
     NEXTGISWEB_URL: "http://hostname.localhost:8080"
     NEXTGISID_URL: "http://hostname.localhost:8081"
     COLLECTOR_HUB_URL: "http://hostname.localhost:8082"
     TRACKER_HUB_URL: "http://hostname.localhost:8083"

     NEXTGISID_INSTANCE_NGID: "00000000-0000-0000-0000-000000000000"
     NEXTGISID_ADMINISTRATOR_NGID: "00000000-0000-0000-0000-000000000000"

   services:
     app:
       image: cr.nextgis.com/nextgisweb/ext/app:3.1.0
       volumes:
         - { type: volume, source: data_app, target: /opt/ngw/data/app }
         - { type: volume, source: config_app, target: /opt/ngw/config/app }
         - { type: volume, source: secret, target: /opt/ngw/secret }
         - { type: volume, source: backup, target: /opt/ngw/backup }
       depends_on: [postgres]
       restart: unless-stopped
       ports:
         - 8080:8080
       environment:
         <<: *shared
         NEXTGISWEB__CORE__LOCALE__DEFAULT: "ru"

     postgres:
       image: cr.nextgis.com/nextgisweb/ext/postgres:3.1.0
       volumes:
         - { type: volume, source: data_postgres, target: /opt/ngw/data/postgres }
         - { type: volume, source: config_postgres, target: /opt/ngw/config/postgres }
         - { type: volume, source: secret, target: /opt/ngw/secret }
       restart: unless-stopped

     archivist:
       image: cr.nextgis.com/nextgisweb/ext/archivist:3.1.0
       volumes:
         - { type: volume, source: data_app, target: /opt/ngw/data/app }
         - { type: volume, source: data_postgres, target: /opt/ngw/data/postgres }
         - { type: volume, source: data_ngid, target: /opt/ngid/data/ngid }
         - { type: volume, source: config_app, target: /opt/ngw/config/app }
         - { type: volume, source: config_postgres, target: /opt/ngw/config/postgres }
         - { type: volume, source: config_ngid, target: /opt/ngid/config/ngid }
         - { type: volume, source: secret, target: /opt/ngw/secret }
         - { type: volume, source: backup, target: /opt/ngw/backup }
       restart: unless-stopped

     ngid:
       image: cr.nextgis.com/nextgisweb/ext/ngid:3.1.0
       volumes:
         - { type: volume, source: data_ngid, target: /opt/ngid/data/ngid }
         - { type: volume, source: config_ngid, target: /opt/ngid/config/ngid }
         - { type: volume, source: secret, target: /opt/ngid/secret }
       restart: unless-stopped
       ports:
       - 8081:8080
       environment:
         <<: *shared
         NGID_ONPREMISE_CONFIGURATION: |-
           eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9....

     chub:
       image: cr.nextgis.com/nextgisweb/ext/chub:3.1.0
       volumes:
         - { type: volume, source: secret, target: /opt/chub/secret }
       depends_on: [postgres]
       ports:
         - 8082:8080
       restart: unless-stopped
       environment:
         <<: *shared

     thub:
       image: cr.nextgis.com/nextgisweb/ext/thub:3.1.0
       volumes:
         - { type: volume, source: secret, target: /opt/thub/secret }
       depends_on: [postgres]
       restart: unless-stopped
       ports:
         - 8083:8080
       environment:
         <<: *shared

   volumes:
     data_app: {}
     data_ngid: {}
     data_postgres: {}
     config_app: {}
     config_ngid: {}
     config_postgres: {}
     secret: {}
     backup: {}

Конфигурация обратного прокси-сервера Nginx
-------------------------------------------

.. code:: nginx

   # Переменная $http_upgrade должна быть определена в корневом блоке http,
   # см. https://nginx.org/en/docs/http/websocket.html
   map $http_upgrade $proxy_connection {
       default upgrade;
       ''      close;
   }

   # Блок виртуального сервера для NextGIS Web
   server {
       server_name ngw.example.com;

       # Директивы сервера: listen, ssl_* и т.д.

       location / {
           client_max_body_size 0;
           proxy_read_timeout 3600s;

           proxy_http_version 1.1;
           proxy_pass http://localhost:8080;
           proxy_set_header Host $http_host;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection $proxy_connection;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_set_header X-Forwarded-For $remote_addr;
       }
   }

   # Блок виртуального сервера для NextGIS ID
   server {
       server_name ngid.example.com;

       # Директивы сервера: listen, ssl_* и т.д.

       location / {
           client_max_body_size 0;

           proxy_http_version 1.1;
           proxy_pass http://localhost:8081;
           proxy_set_header Host $http_host;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection $proxy_connection;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_set_header X-Forwarded-For $remote_addr;
       }
   }

   # Блок виртуального сервера для NextGIS Collector Hub
   # Используется только в Extended редакции
   server {
       server_name chub.example.com;

       # Директивы сервера: listen, ssl_* и т.д.

       location / {
           client_max_body_size 0;

           proxy_http_version 1.1;
           proxy_pass http://localhost:8082;
           proxy_set_header Host $http_host;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection $proxy_connection;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_set_header X-Forwarded-For $remote_addr;
       }
   }

   # Блок виртуального сервера для NextGIS Tracker Hub
   # Используется только в Extended редакции
   server {
       server_name thub.example.com;

       # Директивы сервера: listen, ssl_* и т.д.

       location / {
           client_max_body_size 0;

           proxy_http_version 1.1;
           proxy_pass http://localhost:8083;
           proxy_set_header Host $http_host;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection $proxy_connection;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_set_header X-Forwarded-For $remote_addr;
       }
   }
