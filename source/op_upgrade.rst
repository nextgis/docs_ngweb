Upgrade
==========

.. note:: NextGIS team can perform the upgrade remotely, if you provide remote access that fits the `requirements <https://docs.nextgis.com/docs_ngweb/source/remote.html>`_.

The version number for NextGIS Web On-Premise contains three parts in ``A.B.C`` pattern, where ``A.B`` is the main version number and ``C`` is a patch version. Upgrades between main versions must be performed step by step, without skipping a version. So if your current version is ``2.8.0`` and you wish to update to ``3.0.0``, you need to update to ``2.9.0`` first, and only then to ``3.0.0``. If there are patch versions (e.g. ``2.8.1``, ``2.8.2`` and so on), update to the latest patch version of the main version.

The Administrator can check what the current version of NextGIS Web On-Premise is by viewing the System information section of the Control panel. Also it is present in the ``docker-compose.yaml`` file in the image names (``services.<service_name>.image``). 

.. important:: All steps in this section must be performed by the ``root`` user. If you use ``sudo`` to avoid mixing up the commands, we recommend first running ``sudo -i`` to get a fully functional root user session.

If you are upgrading with Internet access, to obtain the images you must authenticate to the NextGIS Container Registry using the login (example) and password (sesame) provided by NextGIS during the initial installation:

.. code:: bash

   $ docker login cr.nextgis.com -u example -p sesame
   Login Succeeded

To update from one version to another follow these steps:

**Step 1.** Review the actions and requirements listed below for the specific version:

.. contents::
   :local:
   :depth: 1

**Step 2.** Create an offline backup of the system as described in the "Backups" section.

**Step 3.** Create a backup of the file ``docker-compose.yaml`` (replace ``A.B.C`` in the command with the number of your current version): 

.. code:: bash

   $ cd /srv/ngwdocker
   $ cp docker-compose.yaml docker-compose-A.B.C.yaml

**Step 4.** Update ``docker-compose.yaml`` by changing the version in the image names (``services.<service_name>.image``) to the next version you wish to update to.

**Step 5.** Load the new Docker images to your server. If you have Internet access, run the command:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose pull

If you are upgrading without Internet access, at this stage you need to import the Docker images of the new version to the server. Generate an archive of the images using the `NextGIS Container Registry image export <https://toolbox.nextgis.com/t/ngcr_export>`_ tool. You can run the export tool using any NextGIS ID account, including the free plan, but when running the tool you will need to provide your login and password for access to the NextGIS Container Registry. Download the image archive to your local computer, then transfer it to the server and run the command to load images from the archive:

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

Upgrade to 3.3.x from 3.2.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Upgrade to 3.2.x from 3.1.x
-----------------------------------------

Step 6 for Extended Edition (including Whitelabel) requires starting the ``chub`` service with ``docker compose up -d chub`` before running ``docker compose run --rm app nextgisweb maintenance``. The commands for step 6 are as follows:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm app nextgisweb migration upgrade --no-dry-run
   $ docker compose up -d chub
   $ docker compose run --rm app nextgisweb maintenance

Upgrade to 3.1.x from 3.0.x
-----------------------------------------

In version 3.1.0, the system requirements have changed. Ensure that your system meets the requirements listed below:

- Minimum Docker Engine version - 23.0.0.
- Minimum Docker Compose version - 2.x.
- Minimum Linux kernel version - 5.10.
- Support for Ubuntu 20.04 has been discontinued.

Before updating check the collation of the PostgreSQL databases by running the following command:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose exec postgres psql -c "SELECT datname, datcollate, datctype FROM pg_database  WHERE datcollate <> 'C.UTF-8' OR datctype <> 'C.UTF-8'"
    datname | datcollate | datctype
   ---------+------------+----------
   (0 rows)

If the result is empty (0 rows, as in the example), you can continue with the update. 

If it's not empty, you must not continue. Contact NextGIS support instead. It may happen if the initial installation had the version below 1.7.0 (released in August 2021).

Upgrade to 3.0.x from 2.9.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Upgrade to 2.9.x from 2.8.x
-----------------------------------------

After creating a backup (step 3) you need to update PostGIS by running the commands cited below. As a result an interactive PostgreSQL console opens inside the container of the NextGIS Web database:

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

Upgrade to 2.8.x from 2.7.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Upgrade to 2.7.x from 2.6.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Upgrade to 2.6.x from 2.5.x
-----------------------------------------

No additional steps needed, proceed with the standard steps to update.

Upgrade to 2.5.x from earlier versions
------------------------------------------------

Depending on the version you are upgrading from, additional steps may be required. Please contact NextGIS support for recommendations on upgrading to version 2.5.x. Attach the current version of the file ``docker-compose.yaml`` from your server to the support request.
