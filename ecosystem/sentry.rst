.. _ecosystem-sentry:

Sentry
======

Da die ioBroker-Installation lokal bei den Nutzern auf eigener Hardware läuft (und nicht als Cloud-Installation), ist es schwierig seltene Fehler in Adaptern (frühzeitig) zu erkennen. Aus diesem Grund gibt es Lösungen wie `sentry.io <https://sentry.io/>`_, welche Fehler an eine zentrale Stelle melden.

Mit diesem Dienst (falls aktiviert), werden unbehandelte Fehler direkt an das ioBroker-Team gemeldet. Neben der eigentlichen Fehlermeldung werden noch weitere Informationen, wie die aktuelle Adapter-Version, die ``js-controller`` Version, das Betriebssystem und mehr hilfreiche Infos übermittelt. Mit diesen Daten kann der Fehler dann im Idealfall identifiziert und behoben werden.

.. note::
    Nicht alle Adapter sind für Sentry konfiguriert. In der Admin-Oberfläche sieht man als Nutzer, welche Adapter von Sentry überwacht werden. Sollte man die Übermittlung nicht wünschen, kann man das Plugin deaktivieren.

Das Sentry-Plugin wird vom ``js-controller`` als Abhängigkeit automatisch mit installiert.

Konfiguration (dev)
-------------------

Die Konfiguration findet in der :ref:`development-iopackage` (``common.plugins.sentry``) statt.

Nachdem ein neues Projekt auf `sentry.iobroker.net <https://sentry.iobroker.net/>`_ (von einem Admin) angelegt wurde, muss der entsprechende DSN im Adapter konfiguriert werden. Beispiel:

.. code:: json

    "plugins": {
        "sentry": {
            "dsn": "https://baf35e4e423d409bbec94cb01b55257e@sentry.iobroker.net/103"
        }
    }

Weitere Optionen werden ebenfalls unterstützt (siehe Dokumentation).

Im Standard werden alle unbehandelten Fehler (unhandled exceptions) an Sentry weitergeleitet. Zusätzlich können auch eigene Events an Sentry gesendet werden. Beispiele gibt es ebenfalls in der Dokumentation.

Das Sentry-Plugin wird dabei vom Paket ``@iobroker/js-controller-adapter`` als Abhängigkeit geladen und ist daher immer verfügbar.

Konfiguration (User)
--------------------

Als Nutzer kann man im Admin über die Experten-Einstellungen auf jeder Instanz Sentry manuell deaktivieren. In diesem Fall wird auf dem Instanz-Objekt ``common.disableDataReporting = true`` gesetzt.

Damit läuft das Plugin zwar weiter, aber sendet keine Daten mehr.

Möchte man das Plugin komplett deaktivieren bzw. nicht mehr starten, kann dies über den Zustand ``system.adapter.<adapter>.0.plugins.sentry.enabled`` gesteuert werden. Steht dieser Wert auf ``false``, startet der ``js-controller`` das Plugin für diese Instanz gar nicht mehr.

Um das Plugin für den kompletten Host bzw. alle Instanzen auf dem Host zu deaktivieren, kann der Zustand ``system.host.<host>.plugins.sentry.enabled`` verwendet werden.

Links
-----

- `sentry.iobroker.net <https://sentry.iobroker.net/>`_
- `Sentry-Plugin Repository <https://github.com/ioBroker/plugin-sentry>`_ - ``@iobroker/plugin-sentry``
- `Plugin-Base Repository <https://github.com/ioBroker/plugin-base>`_ - ``@iobroker/plugin-base``
