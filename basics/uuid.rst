.. _basics-uuid:

System-UUID
===========

Jede Installation bekommt automatisch eine zufällige UUID zugewiesen, welche für unterschiedliche Aktionen genutzt wird. Dieses kann beispielsweise über das :ref:`basics-cli` ausgelesen werden:

.. code:: console

    iobroker uuid

Diese UUID wird unter anderem für folgende Informationen/Dienste genutzt:

- :ref:`ecosystem-statistics`
- :ref:`ecosystem-ratings`

Weiterhin dient die UUID unter anderem zur Lizenzierung für kostenpflichtige Adapter.

Speicherort
-----------

Wie alles im ioBroker, ist auch die UUID in einem Objekt gespeichert. Über den Admin-Adapter kann das Objekt im Expertenmodus angeschaut werden. Zu finden im Objekt mit der ID `system.meta.uuid`.
