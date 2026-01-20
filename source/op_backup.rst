Резервное копирование
=====================

В NextGIS Web On-Premise реализовано два механизма резервного копирования: 

-  Офлайн - требует остановки всех сервисов и выполняется снаружи    контейнера приложения. Рекомендуется использовать для создания полных резервных копий при переносе на другой сервер.

-  Онлайн - не требует остановки сервисов и выполняется внутри    контейнера приложения. Рекомендуется использовать для регулярного    резервного копирования.

..

.. important:: Все шаги в этом разделе должны выполняться с правами    суперпользователя (``root``). Если вы используете ``sudo``, то чтобы    не запутаться в командах, рекомендуется сначала выполнить ``sudo -i``    для получения полноценной сессии суперпользователя.

.. _backup_path:

Расположение резервных копий
----------------------------

При использовании обоих механизмов резервного копирования, созданные резервные копии сохраняются в вольюме ``backup``, который смонтирован в директорию ``/opt/ngw/backup`` внутри контейнеров. В зависимости от конфигурации физически файлы резервных копий могут находиться, либо в ``/var/lib/docker/volumes/ngwdocker_backup/_data``, либо в директории ``/srv/ngwdocker/backup``, если в файле ``docker-compose.yaml`` для вольюма ``backup`` указан монтируемый путь.

.. _backup_offline:

Офлайн резервное копирование
----------------------------

Этот механизм реализован при помощи вспомогательного сервиса ``archivist``, который входит в состав стека Docker NextGIS Web On-Premise. Суть его работы довольно проста: при помощи GNU Tar создает архив со данными всех вольюмов Docker используемых в стеке.

Резервные копии сделанные при помощи этого механизма предназначены для восстановления на той же версии NextGIS Web On-Premise, с которой была создана резервная копия.

.. _backup_offline_create:

Создание резервной копии
~~~~~~~~~~~~~~~~~~~~~~~~

Для создания консистентной копии необходимо, чтобы никакие сервисы не меняли данные в момент создания резервной копии, поэтому перед созданием резервной копии необходимо остановить все сервисы стека:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose down

После остановки стека выполните команду создания резервной копии:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm archivist backup
   Container ngwdocker-archivist-run-ff7e60e7c67f Creating
   Container ngwdocker-archivist-run-ff7e60e7c67f Created
   backup/archivist-20260117-002345.tar.zst

Файл ``archivist-20260117-002345.tar.zst`` имя которого содержит дату и время создания - это и есть созданная резервная копия. Посмотреть содержимое директории с резервными копиями можно например так:

.. code:: bash

   $ ls -lhn /var/lib/docker/volumes/ngwdocker_backup/_data/
   total 6.8M
   -rw------- 1 1000 1000 6.8M Jan 17 03:23 archivist-20260117-002345.tar.zst

Посмотреть список всех файлов в архиве можно например так:

.. code:: bash

   $ tar -tvf /var/lib/docker/volumes/ngwdocker_backup/_data/archivist-20260117-002345.tar.zst

.. _backup_offline_restore:

Восстановление из резервной копии
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Восстановление осуществляется при помощи того же сервиса ``archivist``.
Для восстановления необходимо остановить все сервисы стека:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose down

После чего выполнить команду восстановления из резервной копии, указав путь к файлу резервной копии в том виде, в котором он был выведен при создании резервной копии (``backup/*.tar.zst``):

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm archivist restore backup/archivist-20260117-002345.tar.zst
   Container ngwdocker-archivist-run-2ab5bee644f6 Creating
   Container ngwdocker-archivist-run-2ab5bee644f6 Created

После завершения восстановления можно запустить стек заново:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose up -d

.. _backup_online:

Онлайн резервное копирование
----------------------------

Онлайн резервное копирование обеспечивается встроенным механизмом NextGIS Web и выполняется внутри контейнера приложения. Этот механизм не требует остановки сервисов и технически представляет собой дамп БД PostgreSQL и архив с файлами данных NextGIS Web.

Резервные копии сделанные при помощи этого механизма предназначены для восстановления на той же версии NextGIS Web On-Premise, с которой была создана резервная копия.

.. _backup_online_create:

Создание резервной копии
~~~~~~~~~~~~~~~~~~~~~~~~

Для создания резервной копии необходимо выполнить команду:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm app nextgisweb backup
   Container ngwdocker-app-run-36f652b37a86 Creating
   Container ngwdocker-app-run-36f652b37a86 Created
   backup/nextgisweb-20260117-131917.ngwbackup

Как и в случае офлайн резервного копирования при помощи ``archivist``, имя файла резервной копии содержит дату и время создания. Файл сохраняется в директории ``/opt/ngw/backup`` внутри контейнера приложения, которая смонтирована на вольюм ``backup`` стека Docker. Если посмотреть содержимое директории с резервными копиями, то можно увидеть созданный файл:

.. code:: bash

   $ ls -lhn /var/lib/docker/volumes/ngwdocker_backup/_data/
   total 210K
   -rw-r--r-- 1 1000 1000 210K Jan 17 16:19 nextgisweb-20260117-131917.ngwbackup

.. _backup_online_restore:

Восстановление из резервной копии
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Для восстановления из онлайн-резервной копии необходимо остановить все сервисы стека и запустить только сервис ``postgres``:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose down
   $ docker compose up -d postgres

Далее необходимо дождаться запуска сервиса ``postgres``, обычно это занимает от 10 до 30 секунд, и удалить БД ``nextgisweb`` и перезапустить сервис ``postgres`` еще раз, чтобы чистая база данных была создана заново. Так же необходимо удалить вольюм с файлами данных NextGIS Web:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose exec postgres psql -c "DROP DATABASE nextgisweb"
   $ docker compose up -d --force-recreate postgres
   $ docker volume rm ngwdocker_data_app

На этом шаге также нужно дождаться запуска сервиса ``postgres``, обычно это происходит в течение 10-30 секунд. После чего можно выполнить восстановление из резервной копии, указав путь к файлу резервной копии в том виде, в котором он был выведен при создании резервной копии (``backup/*.ngwbackup``):

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose run --rm app nextgisweb restore backup/nextgisweb-20260117-131917.ngwbackup
    ✔ Volume ngwdocker_data_app      Created
    ✔ Container ngwdocker-postgres-1 Running
   Container ngwdocker-app-run-10fb82472656 Creating
   Container ngwdocker-app-run-10fb82472656 Created

После завершения восстановления можно запустить стек заново:

.. code:: bash

   $ cd /srv/ngwdocker
   $ docker compose up -d
