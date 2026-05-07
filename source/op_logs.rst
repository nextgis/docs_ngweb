How to get docker container logs
================================

If a problem occurs in NextGIS Web or NextGIS GeoServices , it's important to have full information on what was going on in the program at the time. To provide the support team with this information you can use logs and debug messages.

We recommend the following way to gather logs. It's the standard procedure to get logs in docker compose. 

Gather logs for NextGIS Web
----------------------------

.. important:: Must be performed by the Administrator of NextGIS Web on-premise. 


To gather information necessary for diagnostics, please do the following:

1. Reproduce the issue - repeat the actions that lead to the error, make sure the error is ocurring.
2. After that, on your server go to ``/srv/ngwdocker`` and find the configuration file ``docker-compose.yaml``. 
3. In command line interface run the command cited below to gather logs for the last 24 hours. This interval guarantees that the information about the issue is included:

.. code::

    docker compose logs --since 24h app > app_logs_$(date +%Y-%m-%d).log

4. Send the resulting log file to support@nextgis.com so that the developers can study it.

Gather logs for NextGIS GeoServices
------------------------------------

.. important:: Must be performed by the Administrator of NextGIS GeoServices on-premise. 

We recommend the following way to gather logs. It's the standard procedure to get logs in docker compose. 

To gather information necessary for diagnostics, please do the following:

1. Reproduce the issue - repeat the actions that lead to the error, make sure the error is ocurring.
2. After that, on your server go to ``/srv/geoservices`` and find the configuration file ``docker-compose.yaml``. 
3. In command line interface run the command cited below to gather logs of the app container for the last 24 hours. This interval guarantees that the information about the issue is included.

Example, for the ``app`` container (if you need logs for another container, insert the corresponding argument instead):

.. code::

    docker compose logs --since 24h app > app_logs_$(date +%Y-%m-%d).log

4. Send the resulting log file to support@nextgis.com so that the developers can study it.
