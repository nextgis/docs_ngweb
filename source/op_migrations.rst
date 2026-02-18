Процесс применения миграций при обновлении NextGIS Web
======================================================

1. Заранее загрузите docker-compose

.. code::

    docker-compose pull

Если нет, то при попытке использовать будет попытка скачать.

2. Поменяйте docker-compose.yaml на новый репозиторий и версию 2.7.0.

3. Запустите bash из app в однократном режиме:

.. code::

    docker-compose run --rm app bash


4. Внутри app утилита nextgisweb:


.. code::

    $ nextgisweb migration upgrade


Примените:


.. code::

    $ nextgisweb migration upgrade --no-dry-run


    $ exit


5. Выйдите и запустите новое:


.. code::

    docker-compose up -d
