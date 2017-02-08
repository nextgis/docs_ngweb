.. sectionauthor:: Максим Дубини <maxim.dubinin@nextgis.ru>

.. _ngw_install_qgis:

Установка NextGIS Web QGIS
============================

Для установки модуля рендеринга с помощью QGIS необходимо, чтобы в системе уже был установлен QGIS версии 2.8 и выше.

.. code:: bash

    cd ~/ngw
    git clone git@github.com:nextgis/nextgisweb_qgis.git
    source env/bin/activate
    pip install -e nextgisweb_qgis/

QGIS и зависимости PyQT4 не перечисляются в ``setup.py`` потому что их сложно устанавливать в virtualenv. Поэтому просто копируем эти библиотеки из системных пакетов в virtualenv. Обычно они находятся в пакетах ``python-sip``, ``python-qt4`` и ``python-qgis``.

.. code:: bash
    # DST should point to virtualenv site-packages directory.
    # If it is point to another place you have to modify DST definition.
    # For example: DST=`python -c "import sys; print sys.path[-2]"`
    DST=`python -c "import sys; print sys.path[-1]"`
    echo $DST
    cp `/usr/bin/python -c "import sip; print sip.__file__"` $DST
    cp -r `/usr/bin/python -c "import PyQt4, os.path; print os.path.split(PyQt4.__file__)[0]"` $DST
    cp -r `/usr/bin/python -c "import qgis, os.path; print os.path.split(qgis.__file__)[0]"` $DST

Для версии QGIS 2.16 и выше:

.. code:: bash
    # Only for latest QGIS version (2.16 and higher)
    export PYTHONPATH=$PYTHONPATH:/usr/share/qgis/python
    cp -r `/usr/bin/python -c "import PyQt, os.path; print os.path.split(PyQt.__file__)[0]"` $DST
