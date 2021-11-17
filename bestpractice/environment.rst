.. _bestpractice-environment:

DEV-Environment
===============

Um ioBroker-Adapter zu entwickeln, gibt es mehrere Wege. Der einfachste und schnellste ist der ``dev-server``. Dieses Paket wird vom ioBroker-Team bereitgestellt und ermöglicht eine schnelle und einfache Installation der ioBroker-Umgebung. Dieser ermöglicht es, den Adapter mit unterschiedlichen Versionen des ``js-controller`` zu testen.

.. code:: console

    npm install --global @iobroker/dev-server

Beispiel
--------

.. code:: console

    git clone git@github.com:klein0r/ioBroker.birthdays.git
    cd ioBroker.birthdays/
    dev-server setup --jsController 3.3.18 --admin 5.1.28
    dev-server watch

Für eine ausführliche Dokumentation und alle verfügbaren Parameter, schaue bitte in das `GitHub-Repository <https://github.com/ioBroker/dev-server>`_.

Links
-----

- `GitHub-Repo DEV-Server <https://github.com/ioBroker/dev-server>`_