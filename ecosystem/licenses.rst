.. _ecosystem-licenses:

Lizenzen
========

:octicon:`git-branch;1em;sd-text-info` Unterstützt seit ``js-controller`` Version 4.0.15

Einige Adapter können nur mit einer gültigen Lizenz verwendet werden (beispielsweise VIS). Das heißt nicht unbedingt, dass die Verwendung des Adapters auch kostenpflichtig ist. Häufig wird zwischen privater und kommerzieller Nutzung unterschieden, sodass eine private Nutzung zwar kostenlos ist, aber dennoch eine gültige Lizenz erfordert.

Um Lizenzen zu erstellen / zu erwerben, ist ein Konto auf der Webseite `iobroker.net <https://iobroker.net>`_ notwendig.

Aktuell benötigen nur wenige Adapter eine kostenpflichtige Lizenz (z.B. der KNX- oder LCN-Adapter). Außerdem werden nicht alle kostenpflichtigen Adapter über dieses Lizenzsystem zwingend abgewickelt. Adapter-Entwickler können auch eigene Lizenzmodelle hinterlegen.

.. note::
    Das Lizenz-System sieht vor, dass eine Lizenz nur für bestimmte Versionen oder bis zu einem bestimmten Zeitpunkt gültig ist. So könnte eine gekaufte Lizenz beispielsweise für VIS 1.x ein leben lang gültig sein, aber für VIS 2.x müsste eine neue Lizenz erworben werden. Oder die Lizenz wird z.B. auf ein Jahr limitiert. Weiterhin könnten Lizenzen theoretisch auch an ein System gebunden werden (über die UUID).

Der Nachteil dieses Systems ist natürlich, dass man als Entwickler keine eigenen Lizenzen (mit einem eigenen Private Key) generieren kann, da nur das Shop-System auf der ioBroker-Webseite implementiert ist.

Admin-Integration
-----------------

Um die erstellten bzw. erworbenen Lizenzen zu verwalten, können diese ganz einfach über den Admin abgerufen werden. Dazu kann in den Admin-Einstellungen der Benutzername und das Passwort für das Cloud-Konto hinterlegt werden um alle verfügbaren Lizenzen abzufragen. Diese werden dann (wenn die Seite gespeichert wird) im System-Objekt ``system.licenses`` abgelegt. Dafür wird die folgende API genutzt: ``https://iobroker.net:3001/api/v1/licenses``

Beispiel-Request:

.. code:: console

    curl -X GET -u "info@haus-automatisierung.com:xxxYYYzzz" https://iobroker.net:3001/api/v1/licenses

Hier ein Beispiel, wie das Objekt aufgebaut ist (Passwort und Schlüssel geändert):

.. code:: json

    {
        "_id": "system.licenses",
        "type": "config",
        "common": {
            "name": {
            "en": "Licenses from iobroker.net",
            "de": "Lizenzen von iobroker.net",
            "ru": "Лицензии от iobroker.net",
            "pt": "Licenças de iobroker.net",
            "nl": "Licenties van iobroker.net",
            "fr": "Licences de iobroker.net",
            "it": "Licenze da iobroker.net",
            "es": "Licencias de iobroker.net",
            "pl": "Licencje z iobroker.net",
            "zh-cn": "来自 iobroker.net 的许可证"
            }
        },
        "native": {
            "login": "info@haus-automatisierung.com",
            "password": "$/aes-192-cbc:xxxx:yyyy",
            "licenses": [
                {
                    "json": "...",
                    "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
                    "email": "info@haus-automatisierung.com",
                    "product": "iobroker.vis",
                    "version": "<2",
                    "invoice": "free",
                    "uuid": null,
                    "time": "2018-01-29T21:23:41.000Z",
                    "validTill": "0000-00-00 00:00:00"
                }
            ],
            "readTime": "2023-04-13T14:55:11.163Z"
        },
        "acl": {
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator",
            "object": 1604
        },
        "name": "Licenses from iobroker.net",
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1681397693755
    }

Wie man sehen kann, wird im Attribut ``native.login`` der Benutzername und in ``native.password`` das Passwort des Cloud-Kontos gespeichert. Außerdem werden unter ``native.licenses`` alle verfügbaren Lizenzen hinterlegt, sodass verschiedene Adapter prüfen können, ob eine gültige Lizenz existiert.

Links
-----

- `iobroker.net <https://iobroker.net>`_
