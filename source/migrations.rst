Процесс применения миграций при обновлении NextGIS Web
======================================================

1. Заранее загрузите docker-compose

.. code-bloc::

    docker-compose pull

Если нет, то при попытке использовать будет попытка скачать.

2. Поменяйте docker-compose.yaml на новый репозиторий и версию 2.7.0.

3. Запустите bash из app в однократном режиме:

.. code-bloc::

    docker-compose run --rm app bash


4. Внутри app утилита nextgisweb:


.. code-bloc::

    $ nextgisweb migration upgrade


Примените:


.. code-bloc::

    $ nextgisweb migration upgrade --no-dry-run


    $ exit


5. Выйдите и запустите новое:


.. code-bloc::

    docker-compose up -d
