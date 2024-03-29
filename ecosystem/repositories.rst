.. _ecosystem-repositories:

Repositories
============

Welche Adapter zur Verfügung stehen, wird in sogenannten Repositories hinterlegt. Die Liste an verfügbaren Repositories kann man selbst ändern, um zum Beispiel ein externes Repository zu nutzen. In den allermeisten Fällen werden aber ausschließlich die Standard-Repositories genutzt.

Generell gibt es im Standard zwei verschiedene Adapter-Listen (Repositories), welche vom ioBroker-Team angeboten werden:

- ``stable`` (früher auch ``default`` genannt) - wird täglich aktualisiert und hier bereitgestellt: ``https://download.iobroker.net/sources-dist.json``
- ``beta`` (früher auch ``latest`` genannt) - wird täglich aktualisiert und hier bereitgestellt: ``https://download.iobroker.net/sources-dist-latest.json``

Theoretisch ist es möglich, weitere Repositories zu hinterlegen. Seit dem ``js-controller`` in Version 4.x können mehrere Repositories gleichzeitig aktiv sein.

Pflege der Listen
-----------------

Beide Listen werden in `diesem GitHub Repository (ioBroker.repositories) <https://github.com/ioBroker/ioBroker.repositories>`_ gepflegt.

- ``stable`` = ``sources-dist-stable.json``
- ``beta`` (bzw. früher ``latest``) = ``sources-dist.json``

Im ``stable`` werden getestete Adapter-Versionen aufgenommen. Hier wird **für jedes Repository eine genaue Version angegeben**.

Ein Eintrag sieht beispielsweise so aus:

.. code:: json

    "admin": {
        "meta": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/io-package.json",
        "icon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "type": "general",
        "version": "6.3.5"
    }

In diesem Beispiel wird vom Admin-Adapter die Version ``6.3.5`` als stabil (``stable``) definiert.

Es kann gut sein, dass `auf npm <https://www.npmjs.com/package/iobroker.admin?activeTab=versions>`_ mittlerweile neuere Versionen veröffentlicht wurden. Die aktuellste Version wird über das ``beta`` Repository ausgerollt:

Im Gegensatz dazu hat der Eintrag im ``beta`` Repository keine definierte Versionsnummer:

.. code:: json

    "admin": {
        "meta": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/io-package.json",
        "icon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "type": "general"
    }

Im ``beta`` Repository wird immer **die letzte Version zum Update angeboten** (von npm).

Dieses Vorgehen hat den Vorteil, dass man als Adapter-Entwickler genau steuern kann, welche Nutzer welche Version angeboten bekommen. So können neue Versionen zwar veröffentlicht werden, aber "``stable``-Nutzer" werden erst später auf eine neue Version gebracht, wenn diese von vielen "``beta``-Nutzern" bereits getestet wurde.

.. note::
    Es kann vorkommen, dass einige Adapter zwar im beta-Repository vorhanden sind, aber noch nicht im stable-Repository zu finden sind (weil noch in Entwicklung bzw. noch keine stabile Version verfügbar ist)

.. image:: /images/ioBrokerDoku-Repositories.png
    :alt: ioBroker-Repositories

Bereitgestellte Daten
---------------------

Die offiziellen Repositories werden regelmäßig aktualisiert und auf einem separaten Webserver bereitgestellt. Hier wird der Eintrag mit weiteren Informationen aus der :ref:`development-iopackage` der jeweiligen Adapter angereichert.

.. note::
    Die Aktualisierung des beta-Repository findet 2x täglich planmäßig statt (02:00 und 14:00 Uhr UTC). Die Logik dieses Prozesses ist leider nicht öffentlich dokumentiert.

Jedes Repository enthält dabei (neben einer Liste aller verfügbaren Adapter) einige Meta-Informationen im Schlüssel ``_repoInfo``:

.. code:: json

    "_repoInfo": {
        "stable": true,
        "name": {
            "en": "Official stable repository",
            "de": "Offizielles stabiles Repository",
            "ru": "Официальный стабильный репозиторий",
            "pt": "Repositório estável oficial",
            "nl": "Officiële stabiliteit",
            "fr": "Dépôt stable officiel",
            "it": "Repository stabile ufficiale",
            "es": "Repositorio estable oficial",
            "pl": "Repozytorium",
            "zh-cn": "A. 正式稳定存放处"
        },
        "repoTime": "2023-07-05T06:56:41.309Z"
    }

Unser Beispiel-Eintrag für den Admin-Adapter sieht dann wie folgt aus (zur Übersichtlichkeit wurden Übersetzungen in weitere Sprachen aus dem JSON entfernt):

.. code:: json

    "admin": {
        "name": "admin",
        "version": "6.3.5",
        "titleLang": {
            "en": "Admin",
            "de": "Admin"
        },
        "title": "Admin",
        "connectionType": "local",
        "dataSource": "push",
        "news": {
            "6.3.5": {
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
            "uk": "docs/uk/admin.md",
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

Die aktivien Repositories werden dabei im Objekt ``system.config`` im Attribut ``common.activeRepo`` hinterlegt (als Array). Siehe :ref:`basics-systemconfig`.

.. code:: json

    "activeRepo": [
        "beta",
        "test"
    ]

Alle Repositories werden im Objekt ``system.repositories`` verwaltet. Dort wird neben der URL der komplette JSON-Response gespeichert. Aus dieser Quelle lädt der Admin-Adapter dann die Adpater-Liste:

.. code:: json

    {
        "_id": "system.repositories",
        "type": "config",
        "common": {
            "dontDelete": true,
            "name": {
                "de": "System-Repositories",
                "en": "System repositories",
                "es": "Repositorios del sistema",
                "fr": "Référentiels système",
                "it": "Repository di sistema",
                "nl": "Systeemrepository's",
                "pl": "Repozytoria systemowe",
                "pt": "Repositórios do sistema",
                "ru": "Системные репозитории",
                "zh-cn": "系统资料库"
            }
        },
        "native": {
            "repositories": {
                "beta": {
                    "hash": "ef2811d7c70dea4a55635f2e3aeeb399",
                    "json": {},
                    "link": "https://download.iobroker.net/sources-dist-latest.json",
                    "time": "2023-07-05T17:42:49.065Z"
                },
                "stable": {
                    "link": "https://download.iobroker.net/sources-dist.json"
                }
            }
        },
        "acl": {
            "object": 1604,
            "owner": "system.user.admin",
            "ownerGroup": "system.group.administrator"
        },
        "from": "system.adapter.admin.0",
        "user": "system.user.admin",
        "ts": 1688628692517
    }

Update-Prozess
--------------

Die konfigurierten/aktiven Repositories werden regelmäßig geprüft. Dafür wird die jeweils angegebene URL geändert, sodass stattdessen im ersten Schritt eine Hash-Datei abgerufen wird.

.. code:: javascript

    urlOrPath = urlOrPath.replace(/\.json$/, '-hash.json');

So wird also z.B. statt ``https://download.iobroker.net/sources-dist.json`` erstmal ``https://download.iobroker.net/sources-dist-hash.json`` abgerufen. Aktuell hat die Datei folgenden Inhalt:

.. code:: json

    {
        "hash": "2fbf2f0908829d0597a96d04b24c3e0d",
        "date": "2023-07-05T06:56:41.487Z",
        "name": "sources-dist.json"
    }

Dieser Hash wird mit dem aktuellen Hash im Objekt ``system.repositories`` verglichen. Sollte der Hash abweichen, wird die eigentliche JSON-Datei geladen. Dies wurde so gelöst, um den Traffic von tausenden anfragenden Systemen zu reduzieren.

Links
-----

- `Repository <https://github.com/ioBroker/ioBroker.repositories>`_
