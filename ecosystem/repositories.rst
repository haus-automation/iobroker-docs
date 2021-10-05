.. _ecosystem-repositories:

Repositories
============

Welche Adapter zur Verfügung stehen, wird in sogenannten Repositories hinterlegt. Die Liste an verfügbaren Repositories kann man selbst ändern, um zum Beispiel ein externes Repository zu nutzen. In den allermeisten Fällen wird dies aber niemand machen, sondern nur die Standard-Repositories nutzen.

Generell gibt es zwei verschiedene Adapter-Listen (Repositories), welche vom ioBroker-Team angeboten werden:

- ``stable`` - wird täglich aktualisiert und hier bereitgestellt: ``http://download.iobroker.net/sources-dist.json``
- ``beta`` (früher auch ``latest`` genannt) - wird täglich aktualisiert und hier bereitgestellt: ``http://download.iobroker.net/sources-dist-latest.json``

Pflege der Listen
-----------------

Beide Listen werden in `diesem GitHub Repository (ioBroker.repositories) <https://github.com/ioBroker/ioBroker.repositories>`_ gepflegt.

- ``stable`` = ``sources-dist-stable.json``
- ``beta`` bzw. ``latest`` = ``sources-dist.json``

Im ``stable`` werden getestete Adapter aufgenommen. Dort wird neben dem Repository auch eine genaue Version mit angegeben.
Ein Eintrag sieht dort zum Beispiel so aus:

.. code:: json

    "admin": {
        "meta": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/io-package.json",
        "icon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "type": "general",
        "version": "5.1.25"
    }

Wie Du siehst, ist vom Admin-Adapter in diesem Beispiel aktuell die Version ``5.1.25`` als stabil (``stable``) definiert.

Es kann gut sein, dass auf npm mittlerweile neue Versionen vergeben wurden und diese auch veröffentlicht ist. Diese Version bekommt man als Nutzer angeboten, wenn man das ``beta`` Repository wählt.

Im Gegensatz dazu hat der Eintrag im ``beta`` Repository keine definierte Versionsnummer:

.. code:: json

    "admin": {
        "meta": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/io-package.json",
        "icon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "type": "general"
    }

Bei dem ``beta`` Repository wird automatisch immer die letzte freigegebene Version zum Update angeboten (von npm).

Dieses Vorgehen hat den Vorteil, dass man als Adapter-Entwickler genau steuern kann, welche Nutzer welche Version angeboten bekommen. So können neue Versionen zwar veröffentlicht werden, aber "``stable``-Nutzer" werden erst später auf eine neue Version gebracht, wenn diese von vielen "``beta``-Nutzern" bereits getestet wurde.

.. note::
    Es kann vorkommen, dass einige Adapter zwar im beta-Repository vorhanden sind, aber noch nicht im stable-Repository zu finden sind (weil noch in Entwicklung bzw. noch keine stabile Version verfügbar ist)!

.. image:: /images/ioBrokerDoku-Repositories.png
    :alt: ioBroker-Repositories

Bereitgestellte Daten
---------------------

Die offiziellen Repositories werden regelmäßig aktualisiert und auf einem separaten Webserver bereitgestellt. Hier wird der Eintrag mit weiteren Informationen aus der :ref:`development-iopackage` angereichert.

Unser Beispiel-Eintrag für den Admin-Adapter sieht dann wie folgt aus (zur Übersichtlichkeit wurden Übersetzungen in weitere Sprachen aus dem JSON gelöscht):

.. code:: json

    "admin": {
        "name": "admin",
        "version": "5.1.25",
        "titleLang": {
            "en": "Admin",
            "de": "Admin"
        },
        "title": "Admin",
        "connectionType": "local",
        "dataSource": "push",
        "news": {
            "5.1.25": {
                "en": "Corrected some errors reported via sentry and the github issues",
                "de": "Einige Fehler, die Ã¼ber Wache und die Github-Probleme gemeldet wurden, korrigiert"
            },
            "5.1.24": {
                "en": "Corrected some errors reported via sentry and the github issues",
                "de": "Einige Fehler, die Ã¼ber Wache und die Github-Probleme gemeldet wurden, korrigiert"
            },
            "5.1.23": {
                "en": "Corrected some errors reported via sentry",
                "de": "Einige Fehler behoben, die Ã¼ber die Wache gemeldet wurden"
            },
            "5.1.22": {
                "en": "Corrected some errors reported via sentry",
                "de": "Einige Fehler behoben, die Ã¼ber die Wache gemeldet wurden"
            },
            "5.1.21": {
                "en": "Corrected some errors reported via sentry",
                "de": "Einige Fehler behoben, die Ã¼ber die Wache gemeldet wurden"
            },
            "5.1.20": {
                "en": "Corrected some errors reported via sentry",
                "de": "Einige Fehler behoben, die Ã¼ber die Wache gemeldet wurden"
            }
        },
        "desc": {
            "en": "The configuration of ioBroker via Web-Interface",
            "de": "Die Konfiguration von ioBroker Ã¼ber das Web-Interface"
        },
        "docs": {
            "en": "docs/en/admin.md",
            "ru": "docs/ru/admin.md",
            "de": [
                "docs/de/admin.md",
                "docs/de/admin/tab-adapters.md",
                "docs/de/admin/tab-instances.md",
                "docs/de/admin/tab-objects.md",
                "docs/de/admin/tab-states.md",
                "docs/de/admin/tab-groups.md",
                "docs/de/admin/tab-users.md",
                "docs/de/admin/tab-events.md",
                "docs/de/admin/tab-hosts.md",
                "docs/de/admin/tab-enums.md",
                "docs/de/admin/tab-log.md",
                "docs/de/admin/tab-system.md"
            ],
            "pt": "docs/pt/admin.md",
            "nl": "docs/nl/admin.md",
            "es": "docs/es/admin.md",
            "fr": "docs/fr/admin.md",
            "it": "docs/it/admin.md",
            "pl": "docs/pl/admin.md",
            "zh-cn": "docs/zh-cn/admin.md"
        },
        "materialize": true,
        "mode": "daemon",
        "platform": "Javascript/Node.js",
        "loglevel": "info",
        "icon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "messagebox": true,
        "enabled": true,
        "extIcon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "keywords": [
            "setup",
            "config",
            "update",
            "upgrade",
            "system",
            "konfiguration",
            "administration",
            "einrichtung",
            "wartung"
        ],
        "compact": true,
        "readme": "https://github.com/ioBroker/ioBroker.admin/blob/master/README.md",
        "authors": [
            "bluefox &lt;bluefox@ccu.io&gt;",
            "hobbyquaker &lt;hq@ccu.io&gt;"
        ],
        "dependencies": [
            {
                "js-controller": ">=3.2.16"
            }
        ],
        "type": "general",
        "license": "MIT",
        "logTransporter": true,
        "stopBeforeUpdate": true,
        "wwwDontUpload": true,
        "nogit": true,
        "welcomeScreenPro": {
            "link": "admin/index.html",
            "name": "Admin",
            "img": "admin/img/admin.png",
            "color": "pink",
            "order": 5,
            "localLinks": "_default",
            "localLink": true
        },
        "localLinks": {
            "_default": {
                "link": "%protocol%://%bind%:%port%",
                "pro": true
            }
        },
        "plugins": {
            "sentry": {
                "dsn": "https://9d2aaf29332a4999b133c693f43203b9@sentry.iobroker.net/18"
            }
        },
        "jsonConfig": true,
        "adminUI": {
            "config": "json"
        },
        "node": ">=10.0.0",
        "meta": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/io-package.json",
        "published": "2014-12-04T18:45:44.907Z",
        "versionDate": "2021-08-15T12:14:58.829Z",
        "stars": 232,
        "stat": 49433,
        "issues": 118,
        "score": 1,
        "weekDownloads": 6687,
        "repoTime": "2021-10-05T02:19:59.616Z",
        "latestVersion": "5.1.25"
    }

Einstellungen im ioBroker
-------------------------

Der ioBroker kann zwar mehrere Repositories verwalten (zum Beispiel über den Admin-Adapter), aber nur ein einzelnes Repository kann aktiv sein.

Das aktive Repository wird dabei im Objekt ``system.config`` im Attribut ``common.activeRepo`` hinterlegt. Siehe :ref:`basics-systemconfig`.

Update-Prozess
--------------

Das konfigurierte/aktive Repository wird regelmäßig geprüft. Dafür wird die jeweils angegebene URL geändert, sodass stattdessen eine Hash-Datei abgerufen wird.

.. code:: javascript

    urlOrPath = urlOrPath.replace(/\.json$/, '-hash.json');

So wird also z.B. statt ``http://download.iobroker.net/sources-dist.json`` erstmal ``http://download.iobroker.net/sources-dist-hash.json`` abgerufen. Aktuell hat die Datei folgenden Inhalt:

.. code:: json

    {
        "hash": "a3276c4275647354fa9f81748dde7941",
        "date": "2021-10-04T14:21:02.483Z",
        "name": "sources-dist.json"
    }

Dieser Hash wird mit dem aktuellen Hash in ``system.repositories`` verglichen. Sollte der Hash abweichen, wird die eigentliche JSON-Datei geladen. Dies wurde so gelöst, um den Traffic von tausenden anfragenden Systemen zu reduzieren.
