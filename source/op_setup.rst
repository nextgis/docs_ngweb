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

