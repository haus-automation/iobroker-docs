.. _basics-compactmode:

Compact Mode
============

Die grunlegende :ref:`basics-architecture` vom ioBroker sieht vor, dass jede Instanz eines Adapters in einem eigenen Prozess gestartet wird. Da dies aber sehr viele Ressourcen benötigt, wurde der sogenannte Compact Mode entwickelt. In diesem Modus werden sämtliche Instanzen von Adaptern, welche diesen Modus unterstützen, im gleichen Prozess gestartet, wie der ``js-controller``.

.. todo::
    Add Compact Mode

- ExtSource: https://forum.iobroker.net/topic/18338/experimentell-js-controller-compact-mode
- ExtSource: https://forum.iobroker.net/topic/32789/anleitung-für-adapter-entwickler-compact-mode-testen
