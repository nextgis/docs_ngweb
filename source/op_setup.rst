Configure
=========

.. important:: All steps in this section must be performed by the ``root`` user. If you use ``sudo`` to avoid mixing up the commands, we recommend first running ``sudo -i`` to get a fully functional root user session.

Change endpoints
------------------

Changing endpoints may be required if the the name of the DNS server where the system is deployed changes, or if you change the settings of the reverse proxy. To modify the endpoints edit the file ``docker-compose.yaml``, you can find it in ``/srv/ngwdocker`` in the ``x-shared`` section. Then restart the stack.

.. code:: bash

   $ cd /srv/ngwdocker
   $ nano docker-compose.yaml
   $ docker compose up -d

Configuring the LLM provider
----------------------------

AI tools in NextGIS Web, such as generating filters for the feature table in natural language, work on the BYOK (Bring Your Own Key) principle. This means that to use these features, you need to provide your own access key to the OpenAI-compatible API of the LLM provider.

You can specify the connection parameters and the model used in ``docker-compose.yaml`` in the ``services.app.environment`` section using the following environment variables:

.. code::

   NEXTGISWEB__LLM_CORE__BASE_URL: "https://api.openai.com/v1"
   NEXTGISWEB__LLM_CORE__API_KEY: "<API key>"
   NEXTGISWEB__LLM_CORE__MODEL: "gpt-4o-mini"

After making changes, you need to restart the stack with the command ``docker compose up -d`` to apply the new settings. After that, AI tools will be available in the NextGIS Web interface.

.. _move_server:

Migrating to another server
---------------------------

Steps:

1. On the **source server**, create an offline backup following the :ref:`Create backup <backup_offline_create>` instructions.

2. On the **new server**, install NextGIS Web following the :doc:`op_install` instructions.

    .. important::

        The version installed on the new server must match the version on the
        source server. Restoring a backup across different versions is not
        supported.

3. Copy the backup file to the new server, for example::

.. code:: bash

   $ scp /var/lib/docker/volumes/ngwdocker_backup/_data/archivist-20260117-002345.tar.zst \
        root@new-server:/var/lib/docker/volumes/ngwdocker_backup/_data/

4. Restore the data from the backup following the :ref:`Restore from backup <backup_offline_restore>` instructions.
