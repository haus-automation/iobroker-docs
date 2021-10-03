.. _ecosystem-statistics:

Statistics
==========

Falls konfiguriert, sendet der ioBroker regelmäßig Nutzungsstatistiken an das ioBroker-Team. So kann ermittelt werden, welche Adapter besonders häufig im Einsatz sind, welche Sprache die Nutzer am meisten einstellen und aus welchem Land die Nutzer sind.

Modus
-----

Hier ein Beispiel, was genau gesendet wird. Dies entspricht dem Modus "erweitert" (`extended`):

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

Weiterhin gibt es die Option "ohne Stadt" (`no-city`), welche das letzte Attribut (city) nicht mitsendet.

Im Modus "normal" (`normal`) werden weniger Informationen übertragen:

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

Backend
-------

Die Daten werden vom `js-controller` an die `http://download.iobroker.net/diag.php` gesendet (POST-Request mit JSON-Payload an `data`).

.. code:: console

    curl -v -X POST -d 'data={"uuid": "xxx","language": "de","hosts": [{"version": "3.3.18","platform": "Javascript/Node.js","type": "linux"}],"node": "v12.22.6","arch": "arm","adapters": {"admin": {"version": "5.1.25","platform": "Javascript/Node.js"},"discovery": {"version": "2.7.0","platform": "Javascript/Node.js"},"backitup": {"version": "2.1.17","platform": "Javascript/Node.js"},"feiertage": {"version": "1.0.17","platform": "javascript/Node.js"}},"statesType": "file","objectsType": "file"}' http://download.iobroker.net/diag.php

