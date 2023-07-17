.. _adapters-api:

API-Adapter
===========

Soll von extern auf den ioBroker zugegriffen werden, können mehrere Adapter genutzt werden

- `ioBroker.rest-api <https://github.com/ioBroker/ioBroker.rest-api>`_
- `ioBroker.simple-api <https://github.com/ioBroker/ioBroker.simple-api>`_
- `ioBroker.web <https://github.com/ioBroker/ioBroker.web>`_

Leider ist es etwas undurchsichtig und im ersten Moment verwirrend, welcher Adapter für was genutzt werden sollte. Noch undurchsichtiger wird es, wenn man die Einstellungen des ``web`` Adapters anschaut - denn dieser stellt bei bedarf eine integrierte ``simple-api`` bereit.

.. note::
    Viele Adapter setzen den web-Adapter voraus bzw. benötigen eine Installation des Adapters. Der Grund dafür sind sogenannte "Extensions", welche den Funktionsumfang des web-Adapters erweitern. Andere Adapter können sich als Plugin im web-Adapter anmelden und so weitere URLs anbieten und weitere Daten ausliefern.

.. tip::
    Auch der rest-api-Adapter kann als Plugin für den web-Adapter gestartet werden. In diesem Fall wird dann nur ein Port (der des web-Adapters) benötigt.

.. warning::
    Zum aktuellen Zeitpunkt wird der simple-api-Adapter nicht weiter entwickelt und wurde vollständig durch den (umfangreicheren) rest-api-Adapter abgelöst.

Ports
-----

Da alle genannten Adapter eigehende Verbindungen von außen (im lokalen Netzwerk) erlauben, sind Portangaben notwendig. Die Standard-Ports der verschiedenen Adapter lauten:

- ``rest-api``: 8093 (falls nicht als web-Adapter-Plugin gestartet)
- ``simple-api``: 8087
- ``web``: 8082

Diese Ports können bei Bedarf in der jeweiligen Instanz-Konfiguration angepasst werden.

Aktionen
--------

**State lesen**

Diese Funktionen geben nicht nur den Wert zurück, sondern das komplette State-Objekt als JSON - siehe :ref:`basics-datastorage`

- ``rest-api``: ``http://iobroker:8093/v1/state/system.adapter.admin.0.alive``
- ``rest-api`` (web-Adapter-Plugin): ``http://iobroker:8082/rest-api/v1/state/system.adapter.admin.0.alive``
- ``simple-api``: ``http://iobroker:8087/get/system.adapter.admin.0.alive`` (+ zugehörige Objektdefinition, siehe unten)
- ``web``: nicht möglich

Beispiel-Reponse simple-api:

.. code:: json

    {
        "val": true,
        "ack": true,
        "ts": 1689585976700,
        "q": 0,
        "from": "system.adapter.admin.0",
        "lc": 1689169647888,
        "expire": true,
        "_id": "system.adapter.admin.0.alive",
        "type": "state",
        "common": {
            "name": "admin.0 alive",
            "type": "boolean",
            "read": true,
            "write": true,
            "role": "indicator.state"
        },
        "native": {},
        "user": "system.user.admin",
        "acl": {
            "object": 1636,
            "state": 1636,
            "file": 1632,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        }
    }

**State-Wert lesen**

- ``rest-api``: ``http://iobroker:8093/v1/state/system.adapter.admin.0.alive/plain``
- ``rest-api`` (web-Adapter-Plugin): ``http://iobroker:8082/rest-api/v1/state/system.adapter.admin.0.alive/plain``
- ``simple-api``: ``http://iobroker:8087/getPlainValue/system.adapter.admin.0.alive``
- ``web``: ``http://iobroker:8082/state/system.adapter.admin.0.alive``

**State schreiben**

- ``rest-api``:
- ``rest-api`` (web-Adapter-Plugin): ``http://iobroker:8082/rest-api/v1/state/0_userdata.0.contact.doorbell?value=true``
- ``simple-api``: ``http://iobroker:8087/set/0_userdata.0.contact.doorbell?value=true``

**State umschalten (toggle)**

- ``rest-api``:
- ``rest-api`` (web-Adapter-Plugin): ``http://iobroker:8082/rest-api/v1/state/0_userdata.0.contact.doorbell/toggle``
- ``simple-api``: ``http://iobroker:8087/toggle/0_userdata.0.contact.doorbell``
