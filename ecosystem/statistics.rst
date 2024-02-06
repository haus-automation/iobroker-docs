.. _ecosystem-statistics:

Statistics
==========

Falls konfiguriert, sendet der ioBroker regelmäßig Nutzungsstatistiken an das ioBroker-Team. So kann ermittelt werden, welche Adapter besonders häufig im Einsatz sind, welche Sprache die Nutzer am meisten einstellen und aus welchem Land die Nutzer sind.

Modus
-----

Welche Daten übermittelt werden, hängt vom jeweils konfigurierten Modus ab.

- normal (``normal``)
- ohne Stadt (``no-city``)
- erweitert (``extended``)

Hier ein Beispiel, was genau gesendet wird. Dies entspricht dem Modus "erweitert" (``extended``):

.. code:: json

    {
        "uuid": "xxx",
        "language": "de",
        "country": "Germany",
        "hosts": [
            {
                "version": "3.3.18",
                "platform": "Javascript/Node.js",
                "type": "linux"
            }
        ],
        "node": "v12.22.6",
        "arch": "arm",
        "adapters": {
            "admin": {
                "version": "5.1.25",
                "platform": "Javascript/Node.js"
            },
            "discovery": {
                "version": "2.7.0",
                "platform": "Javascript/Node.js"
            },
            "backitup": {
                "version": "2.1.17",
                "platform": "Javascript/Node.js"
            },
            "feiertage": {
                "version": "1.0.17",
                "platform": "javascript/Node.js"
            }
        },
        "statesType": "file",
        "objectsType": "file",
        "model": "ARMv7 Processor rev 3 (v7l)",
        "cpus": 4,
        "mem": 4025200640,
        "ostype": "Linux",
        "city": "Custom City"
    }

Weiterhin gibt es die Option "ohne Stadt" (``no-city``), welche das letzte Attribut (city) nicht mitsendet.

Im Modus "normal" (``normal``) werden weniger Informationen übertragen:

.. code:: json

    {
        "uuid": "xxx",
        "language": "de",
        "hosts": [
            {
                "version": "3.3.18",
                "platform": "Javascript/Node.js",
                "type": "linux"
            }
        ],
        "node": "v12.22.6",
        "arch": "arm",
        "adapters": {
            "admin": {
                "version": "5.1.25",
                "platform": "Javascript/Node.js"
            },
            "discovery": {
                "version": "2.7.0",
                "platform": "Javascript/Node.js"
            },
            "backitup": {
                "version": "2.1.17",
                "platform": "Javascript/Node.js"
            },
            "feiertage": {
                "version": "1.0.17",
                "platform": "javascript/Node.js"
            }
        },
        "statesType": "file",
        "objectsType": "file"
    }

**Es wird darum gebeten, den erweiterten Modus zu aktivieren.**

Es wird in allen Fällen die :ref:`basics-uuid` der Installation mit übermittelt. Außerdem wird gesendet, wie Du deine Daten speicherst. Siehe auch :ref:`basics-datastorage`.

Backend
-------

Die Daten werden vom ``js-controller`` an ``http://download.iobroker.net/diag.php`` gesendet (POST-Request mit JSON-Payload an ``data``).

.. code:: console

    curl -v -X POST -d 'data={"uuid": "xxx","language": "de","hosts": [{"version": "3.3.18","platform": "Javascript/Node.js","type": "linux"}],"node": "v12.22.6","arch": "arm","adapters": {"admin": {"version": "5.1.25","platform": "Javascript/Node.js"},"discovery": {"version": "2.7.0","platform": "Javascript/Node.js"},"backitup": {"version": "2.1.17","platform": "Javascript/Node.js"},"feiertage": {"version": "1.0.17","platform": "javascript/Node.js"}},"statesType": "file","objectsType": "file"}' http://download.iobroker.net/diag.php

Statistiken analysieren
-----------------------

Im `Developer Portal <https://www.iobroker.dev>`_ können Statistiken zu jedem Adapter abgefragt werden. Diese werden dann grafisch aufbereitet und gegen eine Zeitachse gelegt. So kann man auf einen Blick sehen, wieviele Installationen die einzelnen Versionen zu einem bestimmten Zeitpunkt existieren.

Mit diesen Infos kann man entscheiden, wann und ob eine Version in das ``stable``-Repository übernommen wird.

.. image:: /images/ioBrokerStatistics-TrashSchedule.png
    :alt: TrashSchedule Adapter Statistics (Developer Portal)

Das Portal nutzt dafür die folgende Url: ``https://www.iobroker.dev/api/adapter/<adapter>/stats``

Die Antwort ist wie folgt aufgebaut:

.. code:: json

    {
        "counts": {
            "2021-04-14T02:08:00.628Z": {
                "total": 6535,
                "versions": {
                    "0.0.10": 48,
                    "0.0.11": 202,
                    "0.0.3": 3,
                    "0.0.5": 48,
                    "0.0.6": 2,
                    "0.0.7": 61,
                    "0.0.8": 2,
                    "0.0.9": 28,
                    "1.0.0": 1,
                    "1.0.1": 2,
                    "1.0.3": 787,
                    "1.0.4": 9,
                    "1.0.5": 3,
                    "1.0.6": 3,
                    "1.1.0": 59,
                    "1.1.1": 5255,
                    "1.1.2": 22
                }
            },
            "2021-04-15T18:44:39.370Z": {
                "total": 6570,
                "versions": {
                    "0.0.10": 49,
                    "0.0.11": 201,
                    "0.0.3": 2,
                    "0.0.5": 48,
                    "0.0.6": 2,
                    "0.0.7": 62,
                    "0.0.8": 2,
                    "0.0.9": 28,
                    "1.0.0": 1,
                    "1.0.1": 2,
                    "1.0.3": 785,
                    "1.0.4": 8,
                    "1.0.5": 3,
                    "1.0.6": 3,
                    "1.1.0": 58,
                    "1.1.1": 5293,
                    "1.1.2": 23
                }
            }
        }
    }
