.. _development-notifications:

Notifications
=============

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` 5.0.14

Um Nachrichten an den Host zu senden, stehen sog. Notifications zur Verfügung.

Konfiguration
-------------

Konfiguration in der :ref:`development-iopackage` (``common.notifications`` und ``common.supportedMessages.notifications``)

.. note::
    Generell sollte mit Notifications sparsam umgegangen werden! Es gibt kaum Gründe, warum diese zum Einsatz kommen sollten.

Struktur
--------

- ``scope`` - Ein existierender Scope aus einer Adapter-Konfiguration. z.B. ``system``
- ``category`` - 
- ``message`` - z.B. ``Hallo!``
- ``instance`` - z.B. ``system.adapter.trashschedule.0``

Senden
------

Um eine neue Notification zu erstellen, wird im Adapter ``registerNotification`` genutzt. Hier wird als erstes der Scope, dann die Category die Nachricht übergeben.

.. code:: javascript

    async this.registerNotification('system', 'securityIssues', 'Das ist ziemlich unsicher konfiguriert!');

Oder alternativ mit ``sendToHost`` (nicht empfohlen!):

.. code:: javascript

    await this.sendToHostAsync(
        `system.host.${this.host}`,
        'addNotification',
        {
            scope: 'system',
            category: 'noDiskSpace',
            message: 'Festplatte bald voll',
            instance: `system.adapter.${this.namespace}`,
        }
    );

.. note::
    Der "Primärschlüssel" je Scope und Category ist die Instance. Das heißt, dass je Kategorie nur ein Eintrag einer Instanz vorhanden sein kann!

Empfangen
---------

Technische Details
------------------

Die Notifications werden im Objekt ``system.host.<host>.notifications.<scope>`` abgelegt. Je Kategorie wird dabei gezählt, wieviele Notifications vorliegen. Beispiel:

.. code:: json

    {
        "securityIssues": {
            "count": 2
        },
        "fsIoErrors": {
            "count": 1
        }
    }

Der Zustand (State) selbst, enthält dabei aber nur diese Informationen und nicht die Notification selbst. Diese werden im Dateisystem in der ``notifications.json`` abelegt! So sind diese auch nach einem Neustart des Systems verfügbar und sind nicht von einer funktionierenden Objekt- oder State-Datenbank abhängig.

Beispiel-Inhalt der Datei:

.. code:: json

    {
        "news": {
            "info": {
                "system.adapter.awtrix-light.0": [
                    {
                        "message": "Kurs-Empfehlung! \n Die besten ioBroker-Kurse gibt es auf haus-automatisierung.com",
                        "ts": 1698149101034
                    }
                ]
            }
        },
        "system": {
            "noDiskSpace": {
                "system.adapter.awtrix-light.0": [
                    {
                        "message": "Festplatte bald voll",
                        "ts": 1698149453306
                    }
                ]
            }
        }
    }

Links
-----

- https://github.com/ioBroker/ioBroker.js-controller/blob/v5.0.14/packages/common-db/src/lib/common/notificationHandler.ts
