Update
==========

The version number for NextGIS Web On-Premise contains three parts in ``A.B.C`` pattern, where ``A.B`` is the main version number and ``C`` is a patch version. Updates between main versions must be performed step by step, without skipping a version. So if your current version is ``2.8.0`` and you wish to update to ``3.0.0``, you need to update to ``2.9.0`` first, and only then to ``3.0.0``. If there are patch versions (e.g. ``2.8.1``, ``2.8.2`` and so on), update to the latest patch version of the main version.

The Administrator can check what the current version of NextGIS Web On-Premise is by viewing the System information section of the Control panel. Also it is present in the ``docker-compose.yaml`` file in the image names (``services.<service_name>.image``). 

.. important:: All steps in this section must be performed by the ``root`` user. If you use ``sudo`` to avoid mixing up the commands, we recommend first running ``sudo -i`` to get a fully functional root user session.

To update from one version to another follow these steps:

**Step 1.** Find your current version in this document and check the algorithm for it, it may contain important additional steps and requirements.

**Step 2.** Create an off-line backup of the system as described in the "Backup" section.

**Step 3.** Create a backup of the file ``docker-compose.yaml`` (replace ``A.B.C`` in the command with the number of your current version): 

.. code:: bash

   $ cd /srv/ngwdocker
   $ cp docker-compose.yaml docker-compose-A.B.C.yaml

**Step 4.** Update ``docker-compose.yaml`` by changing the version in the image names (``services.<service_name>.image``) to the next version you wish to update to.

**Step 5.** Load the new Docker images to your server. If you have Internet access, run the command:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose pull

If you don't have Internet access, contact NextGIS support team to get image archives, upload them to the server, then run the command to load the images from the archives:

.. code:: bash

   $ docker load -i /path/to/file.tar.gz

**Step 6.** Apply migrations, launch maintenance and wait for the following command to complete:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm app nextgisweb migration upgrade --no-dry-run
   $ docker compose run --rm app nextgisweb maintenance

**Step 7.** Start the updated containers, it completes the update process:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose up -d

Update to 3.1.x from 3.0.x
-----------------------------------------

Before starting the update check that the server where the system is installed answers the system requirements.

Before updating check the collation of the PostgreSQL databases by running the following command:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose exec postgres psql -c "SELECT datname, datcollate, datctype FROM pg_database  WHERE datcollate <> 'C.UTF-8' OR datctype <> 'C.UTF-8'"
    datname | datcollate | datctype
   ---------+------------+----------
   (0 rows)

If the result is empty (0 rows, as in the example), you can continue with the update. 

If it's not empty, you must not continue. Contact NextGIS support instead. It may happen if the initial installation had the version below 1.7.0 (released in August 2021).

Update to 3.0.x from 2.9.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Update to 2.9.x from 2.8.x
-----------------------------------------

After creating a backup of the current version you need to update PostGIS by running the commands cited below. As a result an interactive PostgreSQL console opens inside the container of the NextGIS Web database:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose down
   $ docker compose up -d postgres
   $ docker compose exec postgres psql -U postgres nextgisweb

In the interactive PostgreSQL console run the following command (``nextgisweb=#`` is the console's welcome message, it's entered automatically 
and you don't have to add it, only the commands that follow):

.. code:: sql

   nextgisweb=# ALTER USER nextgisweb SUPERUSER;
   nextgisweb=# SET SESSION AUTHORIZATION nextgisweb;
   nextgisweb=# SELECT postgis_extensions_upgrade();
   nextgisweb=# ALTER USER nextgisweb NOSUPERUSER;
   nextgisweb=# \q

Update to 2.8.x from 2.7.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Update to 2.7.x from 2.6.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Update to 2.6.x from 2.5.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.
