Create backup
================

NextGIS Web On-Premise has two options for creating backup: 

- Off-line - requires shutting down all services and is performed outside the application container. It is recommended for creating full backups to be transferred to a different server.

- On-line - does not require shutting down the services and is performed within the app container. Recommended for regular backups.

..

.. important:: All steps in this section must be performed by the ``root`` user. If you use ``sudo`` to avoid mixing up the commands, we recommend first running ``sudo -i`` to get a fully functional root user session.

Path to backups
----------------------------

Both methods create backups in the ``backup`` volume mounted in the ``/opt/ngw/backup`` directory inside the containers. Depending on the configuration, physically the backup files may be stored either in ``/var/lib/docker/volumes/ngwdocker_backup/_data``, or in ``/srv/ngwdocker/backup``, if the file ``docker-compose.yaml`` has a mounted path for ``backup``.

Offline backup
----------------------------

This method uses an additional service ``archivist`` included in the Docker NextGIS Web On-Premise stack. The essence of its work is quite simple: using GNU Tar, it creates an archive with data from all Docker volumes used in the stack.

Backups created using this method are meant to be restored to the same version of NextGIS Web On-Premise where the backup was created.

Create backup
~~~~~~~~~~~~~~~~~~~~~~~~

To create a consistent backup it is essential that no services modify data while the backup copy is being created. So before starting the process, shut down all the services of the stack:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose down

After shutting down the stack run the following command to create backup:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm archivist backup
   Container ngwdocker-archivist-run-ff7e60e7c67f Creating
   Container ngwdocker-archivist-run-ff7e60e7c67f Created
   backup/archivist-20260117-002345.tar.zst

File ``archivist-20260117-002345.tar.zst`` is the backup, its name contains date and time of its creation. You can view the contents of the backup directory, for example:

.. code:: bash

   $ ls -lhn /var/lib/docker/volumes/ngwdocker_backup/_data/
   total 6.8M
   -rw------- 1 1000 1000 6.8M Jan 17 03:23 archivist-20260117-002345.tar.zst

To view the list of all the files in the archive you can use:

.. code:: bash

   $ tar -tvf /var/lib/docker/volumes/ngwdocker_backup/_data/archivist-20260117-002345.tar.zst

Restore from backup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To restore data use the same ``archivist`` service.
To restore data, first shut down all the services of the stack:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose down

Then run the command to restore from backup, entering the path to the backup in the same way as it was output during backup creation (``backup/*.tar.zst``):

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm archivist restore backup/archivist-20260117-002345.tar.zst
   Container ngwdocker-archivist-run-2ab5bee644f6 Creating
   Container ngwdocker-archivist-run-2ab5bee644f6 Created

After the restoration is complete, you can restart the stack:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose up -d

Online backup
----------------------------

Online backup is performed by a built-in mechanism within NextGIS Web container. This mechanism does not require shutting down the services. From the technical point of view, it's a PostgreSQL database dump and an archive with all the data files from NextGIS Web.

Backups created using this method are meant to be restored to the same version of NextGIS Web On-Premise where the backup was created.

.. _backup_online_create:

Create backup
~~~~~~~~~~~~~~~~~~~~~~~~

To create a backup run the command:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm app nextgisweb backup
   Container ngwdocker-app-run-36f652b37a86 Creating
   Container ngwdocker-app-run-36f652b37a86 Created
   backup/nextgisweb-20260117-131917.ngwbackup

The name of the backup file contains date and time of its creation, same as for the backups created with the ``archivist``. The files are stored in ``/opt/ngw/backup``  inside the app container mounted to the ``backup`` volume of Docker stack. If you view the contents of the backup directory, you can see the created file:

.. code:: bash

   $ ls -lhn /var/lib/docker/volumes/ngwdocker_backup/_data/
   total 210K
   -rw-r--r-- 1 1000 1000 210K Jan 17 16:19 nextgisweb-20260117-131917.ngwbackup

.. _backup_online_restore:

Restore from backup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To restore from an online backup, shut down all the services of the stack, then run only the ``postgres`` service:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose down
   $ docker compose up -d postgres

Wait for ``postgres`` to launch, it takes between 10 and 30 seconds normally, then delete the ``nextgisweb`` database and restart ``postgres`` to create a blanck database. Also you need to delete the volume containing NextGIS Web data files:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose exec postgres psql -c "DROP DATABASE nextgisweb"
   $ docker compose up -d --force-recreate postgres
   $ docker volume rm ngwdocker_data_app

Wait for ``postgres`` to start again, it should take between 10 and 30 seconds. Next you can restore the data from the backup. Enter the path to the backup the same way it was output during backup creation (``backup/*.ngwbackup``):

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm app nextgisweb restore backup/nextgisweb-20260117-131917.ngwbackup
    ✔ Volume ngwdocker_data_app      Created
    ✔ Container ngwdocker-postgres-1 Running
   Container ngwdocker-app-run-10fb82472656 Creating
   Container ngwdocker-app-run-10fb82472656 Created

After the restoration is complete, you can restart the stack:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose up -d
