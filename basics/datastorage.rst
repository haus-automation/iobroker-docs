.. _basics-datastorage:

Daten-Speicherung
=================

Um Daten zu speichern, muss zuerst ein (neues) Objekt angelegt werden. Dieses Objekt ist statisch und enthält viele Meta-Daten. Zusätzlich gibt es den dynamischen Zustand (state), welcher den aktuellen Wert hält. Beides Zusammen nennen wir Datenpunkt.

Objekt
------

Ein Objekt beschreibt, welche Informationen genau gespeichert werden können. Das sind hauptsächlich Meta-Informationen, welche den Datenpunkt beschreiben. Das sind zum Beispiel:

- Typ
- Name
- Einheit (z.B. °C oder kWh)
- ...

Objekte werden dabei in einer Hierarchie abgebildet. Also in einer Baumstruktur - genau wie in vielen Dateisystemen. **Als Trennzeichen der Ebenen wird dabei der Punkt benutzt.**

.. note::
    Als Anwender muss man nur sehr wenige Objekte anlegen, welche zum persistenten Ablegen von eigenen Daten dienen. Die meisten Objekte werden von den einzelnen Adaptern automatisch angelegt.

Objekte werden unter Linux als JSON (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/objects.json`` 

State
-----

States werden unter Linux als JSON (Text, UTF-8) in der folgenden Datei abgelegt:

``/opt/iobroker/iobroker-data/states.json`` 

Datenpunkt
----------

Wenn man von einem Datenpunkt spricht, ist die Kombination aus Objekt mit dem zugehörigen State gemeint.

Namespace
---------

Der Namespace (Namensraum) ist ein reservierter Bereich für jeden Adapter.



.. note::
    Als Anwender solltest Du keine eigenen Objekte in Namespaces von Adaptern oder vom System ablegen! Wenn Du eigene States erstellen möchtest, tu dies bitte im Namespace **0_userdata**

Weiterhin gibt es den reservierten Namespace ``system.`` für das System. Dort ist unter anderem folgendes enthalten:

- ``system.config`` - Systemkonfiguration (Sprache, Datumsformat, Verwahrungsort, ...)
- ``system.host.<hostname>``` - js-controller-Prozess
- ``system.repositories`` - Liste der vefügbaren Adpater
- ``system.certificates`` - Konfigurierte Zertifikate
- ``system.meta.`` - Meta-Informationen des Systems
- ``system.user.`` - Benutzer
- ``system.group.`` - Benutzer-Gruppen
- ``system.adapter.<adapter-name>`` - default config of an adapter
- ``system.adapter.<adapter-name>.<instance-nummmer>`` - Informationen zur Instanz (Uptime, Ressourcen, ...)

.. warning::
    Ändere keine Informationen in dem System-Namespace, wenn Du nicht genau weißt, was Du da tust. Als normaler Benutzer gibt es keinen Grund dort etwas ändern. Diese Informationen sind nur für Entwickler relevant!