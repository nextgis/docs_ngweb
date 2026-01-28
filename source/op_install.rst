Installation
============

Before installation make sure that you've contacted NextGIS company and obtained:

- File ``docker-compose.yaml`` - see Additional information for examples, but they don't contain all the necessary parameters.
- Access to NextGIS Container Registry (if installation is done using Internet access) or archived Docker images (if installation is done without Internet).

.. note:: NextGIS team can perform the installation remotely, if you provide remote access that fits the `requirements <https://docs.nextgis.com/docs_ngweb/source/remote.html>`_.



Endpoints
-----------

NextGIS Web On-Premise uses several HTTP (or HTTPS) endpoints: 

- NextGIS Web - the main address for user interaction with the system. Default value: ``http://server.example.com:8080``, where ``server.example.com`` is the domain name or IP address of the server where the system is installed.

- NextGIS ID - authentication service used by NextGIS Web as well as by other NextGIS products. Default value: ``http://server.example.com:8081``.

- NextGIS Collector Hub (only for Extended edition) - additional service for NextGIS Collector mobile app. Default value: ``http://server.example.com:8082``.

- NextGIS Tracker Hub (only for Extended edition) - additional service for tracking apps. Default value: ``http://server.example.com:8083``.

If your IT infrastructure allows for it, it is recommended to set up a reverse proxy for TLS encryption and using HTTPS. It is especially important if the software is to be accessed not just from the local network, but also from the Internet. In that case the endpoints depend on the settings of the reverse proxy. The recommended values are:

-  NextGIS Web - ``https://ngw.example.com``
-  NextGIS ID - ``https://ngid.example.com``
-  NextGIS Collector Hub - ``https://chub.example.com``
-  NextGIS Tracker Hub - ``https://thub.example.com``

The reverse proxy is set up by the client's IT department, it is not a responsibility of NextGIS company. The required parameters are cited below using Nginx as example.

Contact your IT department to choose addresses you wish to use and note them in the table below, you'll need them later. Additionally, the table contains examples of names for environment variables of each endpoint.

===================== ===================== ========
Endpoint              Environment variable   Value
===================== ===================== ========
NextGIS Web           ``NEXTGISWEB_URL``    
NextGIS ID            ``NEXTGISID_URL``     
NextGIS Collector Hub ``COLLECTOR_HUB_URL`` 
NextGIS Tracker Hub   ``TRACKER_HUB_URL``   
===================== ===================== ========

Install and configure Docker
----------------------------

.. important:: All steps in this section must be performed by the ``root`` user. If you use ``sudo`` to avoid mixing up the commands, we recommend first running ``sudo -i`` to get a fully functional root user session.

You need to install latest versions of Docker Engine and Docker Compose. We recommend using the official `installation manual from the Docker Web site <https://docs.docker.com/engine/install/>`__, or the manual for your distribution, provided that it answers the minimum required version. Minimum required versions:

-  Docker Engine >= 23
-  Docker Compose >= 2

If the installation is done via Internet, then to get the images log in to NextGIS Container Registry with the username (example) and password (sesame) provided by NextGIS:

.. code:: bash

   $ docker login cr.nextgis.com -u example -p sesame
   Login Succeeded

Deploy Docker stack
--------------------

.. important:: All steps in this section must be performed by the ``root`` user. If you use ``sudo`` to avoid mixing up the commands, we recommend first running ``sudo -i`` to get a fully functional root user session.

On the server create a directory ``/srv/ngwdocker``, copy to it the file ``docker-compose.yaml`` you've got from NextGIS and open it in a text editor, for example, use ``nano``:

.. code:: bash

   $ mkdir -p /srv/ngwdocker
   $ cd /srv/ngwdocker
   $ cp /path/to/your/docker-compose.yaml .
   $ nano docker-compose.yaml

At the start of the ``docker-compose.yaml`` file you'll find a section called  ``x-shared``. It contains varables where you need to enter the previously chosen endpoints (keep in mind that the addresses are entered without ``/`` at the end), e.g.:

.. code:: yaml

   x-shared: &shared
     NEXTGISWEB_URL: "http://server.example.com:8080"
     NEXTGISID_URL: "http://server.example.com:8081"
     COLLECTOR_HUB_URL: "http://server.example.com:8082"
     TRACKER_HUB_URL: "http://server.example.com:8083"

     NEXTGISID_INSTANCE_NGID: "00000000-0000-0000-0000-000000000000"
     NEXTGISID_ADMINISTRATOR_NGID: "00000000-0000-0000-0000-000000000000"

Keep in mind that ``COLLECTOR_HUB_URL`` and ``TRACKER_HUB_URL`` variables are used only in the Extended edition. If they are not present, you don't have to add them. Variables ``NEXTGISID_INSTANCE_NGID`` and ``NEXTGISID_ADMINISTRATOR_NGID`` are pre-filled in the docker-compose.yaml file provided by NextGIS. After making all the edits save the file and close the text editor.

If the software is deployed to a server without Internet access, at this stage you need to upload all the necessary Docker images to your server. Contact NextGIS support team to get image archives, upload them to the server, then run the command to load the images from the archives: 

.. code:: bash

   $ docker load -i /path/to/file.tar.gz

Now you can start the containers. If you have Internet access, all the necessary images are downloaded from NextGIS Container Registry. Run the following command:

.. code:: bash

   $ docker compose up -d
   [+] up 5/5
    ✔ Network ngwdocker_default       Created
    ✔ Container ngwdocker-ngid-1      Created
    ✔ Container ngwdocker-postgres-1  Created
    ✔ Container ngwdocker-archivist-1 Created
    ✔ Container ngwdocker-app-1       Created

If you use a reverse proxy server for HTTPS, configure it to forward the traffic to the corresponding NextGIS Web ports. See an example of Nginx configuration in the `Additional information <https://docs.nextgis.com/docs_ngweb/source/op_additional.html>`_ section. 

After a little while (up to 1 minute normally) you can access NextGIS Web by entering the browser address bar the the URL you set up in the ``NEXTGISWEB_URL`` variable. To log into the system use the default Administrator credentials: ``administrator`` / ``admin``.

Also you can use the same credentials to access NextGIS ID on the address set up in the environment varialbe ``NEXTGISID_URL``. To check if the endpoints are correct, try logging in to NextGIS Web by clicking the **Sign in with NextGIS ID** button.

Weekly maintenance and backup
------------------------------

.. important:: All steps in this section must be performed by the ``root`` user. If you use ``sudo`` to avoid mixing up the commands, we recommend first running ``sudo -i`` to get a fully functional root user session.

To run weekly backup and maintenance create a file named ``cronjob`` with the contents cited below and make it executable.

.. code:: bash

   $ cat <<EOF > cronjob
   #!/bin/bash
   set -ex
   PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
   docker compose exec -T --env NGWDOCKER_INITIALIZE_DB=no \
       app /opt/ngw/bin/docker-entrypoint \
       bash -c "nextgisweb backup && nextgisweb maintenance"
   EOF
   $ chmod +x cronjob

This script launches online backup for NextGIS Web, then the maintenance task. Next set it to be executed every Sunday at 02:00 via cron, then restart:

.. code:: bash

   $ cat <<EOF > /etc/cron.d/ngwdocker
   0 2 * * 0 root cd /srv/ngwdocker && ./cronjob
   EOF
   $ systemctl restart cron
