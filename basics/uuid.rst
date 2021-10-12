.. _basics-uuid:

System-UUID
===========

Jede Installation bekommt automatisch eine zufällige UUID zugewiesen, welche für unterschiedliche Aktionen genutzt wird. Dieses kann beispielsweise über das :ref:`basics-cli` ausgelesen werden:

.. code:: console

    iobroker uuid

Oder über die Objekt-Datenbank

.. code:: console

    iobroker object get system.meta.uuid

.. code:: json

    {
        "type": "meta",
        "common": {
            "name": "uuid",
            "type": "uuid"
        },
        "ts": 1633092016485,
        "from": "system.host.raspberrypi-iobroker.tools",
        "native": {
            "uuid": "23b1992b-8d91-a4fc-b201-2bd851bdc807"
        },
        "_id": "system.meta.uuid",
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

Diese UUID wird unter anderem für folgende Informationen/Dienste genutzt:

- :ref:`ecosystem-statistics`
- :ref:`ecosystem-ratings`
